Qt 应用程序在 Windows 上，在电脑休眠恢复后或多显示器切换后界面内容异常缩小，这确实是一个常见的兼容性问题，通常与 **高 DPI (High Dots Per Inch) 缩放** 有关。这不是某个特定 Qt 版本的独立 Bug，而更像是 Qt 在处理 Windows 不同 DPI 设置和多显示器环境下的复杂性。

### 可能的原因：

1.  **DPI 缩放感知问题 (DPI Awareness):**
    * **非 DPI 感知 (DPI Unaware):** 应用程序在启动时不会查询系统的 DPI 设置，或者只在启动时查询一次。当 DPI 设置改变（例如，从高 DPI 显示器切换到低 DPI 显示器，或休眠恢复后系统重新应用 DPI 设置）时，应用程序的界面不会随之调整，导致在新的 DPI 环境下看起来过小或过大。
    * **系统 DPI 感知 (System DPI Aware):** 应用程序在启动时根据主显示器的 DPI 设置进行缩放。如果应用程序被拖动到不同 DPI 的显示器上，或者系统 DPI 发生变化，应用程序不会自动重新缩放。
    * **每显示器 DPI 感知 (Per-Monitor DPI Aware):** 这是现代应用程序推荐的模式。应用程序可以识别并适应每个显示器的 DPI 变化，当窗口在不同 DPI 的显示器之间移动时，能够正确地重新缩放。Qt 从 5.6 版本开始支持此模式。

2.  **Qt 的默认行为和 DPI 策略：**
    * Qt 默认情况下可能尝试自动处理 DPI 缩放，但 Windows 的 DPI 机制非常复杂，尤其是涉及到多显示器和动态 DPI 变化时，Qt 的自动处理可能不够完善。
    * 某些 Qt 版本或配置可能在处理非整数缩放因子（如 150%）时存在舍入误差，导致界面元素显示异常。

3.  **布局和尺寸计算问题：**
    * 如果界面设计中大量使用了固定像素值而不是基于布局（`QLayout`）和伸缩策略（`QSizePolicy`），那么在 DPI 变化时，固定大小的元素就无法正确缩放。
    * 某些情况下，即使启用了 DPI 缩放，某些内部计算（如 `QFontMetrics` 获取的字体大小、某些 `QOpenGLWidget` 的尺寸等）可能没有正确地适应新的缩放因子。

4.  **Windows 自身的一些限制或行为：**
    * Windows 在处理应用程序 DPI 变化和多显示器切换时，可能存在一些固有问题，即使是原生应用程序有时也会遇到类似情况。
    * 休眠恢复后，系统可能会重新加载显卡驱动或显示设置，这可能导致应用程序对 DPI 的感知出现问题。

### 解决方案：

解决这类问题通常需要组合使用 Qt 的 DPI 相关设置和良好的界面布局实践。

**1. 启用 Qt 的高 DPI 缩放支持 (推荐):**

这是最重要的一步。在应用程序启动的 `main()` 函数中，在创建 `QApplication` 或 `QGuiApplication` 实例之前，设置高 DPI 缩放属性。

```cpp
#include <QApplication>
#include <QGuiApplication> // For Qt::AA_EnableHighDpiScaling

int main(int argc, char *argv[])
{
    // 启用高DPI缩放。这会告诉Qt根据显示器的像素密度自动缩放界面。
    // 适用于 Qt 5.6 及更高版本。
    QGuiApplication::setAttribute(Qt::AA_EnableHighDpiScaling);

    // 对于 Qt 5.14 及更高版本，可以设置DPI缩放因子舍入策略。
    // Qt::HighDpiScaleFactorRoundingPolicy::PassThrough 可能会在某些非整数缩放因子下改善显示效果。
    // 默认是 Qt::HighDpiScaleFactorRoundingPolicy::Round，可能会导致一些舍入问题。
    QGuiApplication::setHighDpiScaleFactorRoundingPolicy(Qt::HighDpiScaleFactorRoundingPolicy::PassThrough);

    QApplication a(argc, argv); // 或 QGuiApplication a(argc, argv);
    // ... 你的应用程序代码
    return a.exec();
}
```

**2. 使用 `qt.conf` 文件：**

在应用程序的可执行文件（`.exe`）所在的目录下创建一个名为 `qt.conf` 的文件，内容如下：

```ini
[Platforms]
WindowsArguments = dpiawareness=2
```

* `dpiawareness=0`: DPI Unaware (不推荐)
* `dpiawareness=1`: System DPI Aware (不推荐，除非你的应用只在主显示器或所有显示器DPI相同的情况下运行)
* `dpiawareness=2`: Per-Monitor DPI Aware (推荐)

这个设置会覆盖 Qt 应用程序的默认 DPI 感知行为。

**3. 环境变量 (用于测试或特定场景):**

你可以通过设置环境变量来影响 Qt 的 DPI 行为。这通常用于测试或在没有源代码的情况下修改行为。

* `QT_AUTO_SCREEN_SCALE_FACTOR=1`: 启用自动缩放，基于显示器的像素密度。
* `QT_SCALE_FACTOR=2.0` (例如): 定义一个全局缩放因子。这会将整个应用程序（包括字体）缩放到指定因子，通常不推荐用于多显示器环境。
* `QT_SCREEN_SCALE_FACTORS="Screen1Name=1.5;Screen2Name=2.0"`: 为每个屏幕指定缩放因子。主要用于调试或解决 EDID 信息不正确的显示器问题。

**4. 优化界面布局：**

* **使用布局管理器 (Layouts):** 始终使用 `QHBoxLayout`, `QVBoxLayout`, `QGridLayout` 等布局管理器来组织你的 UI 元素，而不是手动设置固定坐标和尺寸。布局管理器会自动调整子控件的大小和位置。
* **使用伸缩策略 (Size Policies):** 为你的控件设置合适的 `QSizePolicy`。例如，`Expanding` 可以让控件在可用空间内尽可能扩展，`Fixed` 则保持固定大小。
* **避免硬编码像素值:** 尽量使用 `QWidgets` 的 `sizeHint()`、`minimumSizeHint()` 等方法，让控件根据其内容和样式自行计算推荐尺寸。如果必须指定尺寸，考虑使用 `QStyle` 的 `pixelMetric` 等方法获取与当前 DPI 相关的尺寸。
* **字体处理:** Qt 通常会正确缩放字体，但如果你手动设置了字体大小，确保使用 `pointSize()` 而不是 `pixelSize()`，因为 `pointSize` 是一个物理测量单位，会更好地适应不同的 DPI。

**5. 强制重绘/更新：**

在某些极端情况下，你可能需要在系统休眠恢复或显示器切换后，强制更新或重绘你的界面。这可以通过监听 `QScreen` 的 `geometryChanged()` 或 `physicalSizeChanged()` 信号，然后调用窗口的 `update()` 或 `repaint()` 方法来实现。但通常，正确启用 DPI 缩放后，Qt 应该能自动处理。

**6. 更新 Qt 版本：**

虽然不是特定版本的 Bug，但 Qt 团队一直在改进 DPI 缩放支持。如果你使用的是较老的 Qt 版本，升级到最新的 LTS 版本（如 Qt 5.15 或 Qt 6.x）可能会带来更好的 DPI 兼容性。

### 诊断步骤：

1.  **确认应用程序的 DPI 感知级别：**
    在你的应用程序运行时，打开任务管理器，找到你的应用程序进程，右键点击 -> "详细信息" -> 添加 "DPI 感知" 列。这会显示你的应用程序是 "非 DPI 感知"、"系统 DPI 感知" 还是 "每显示器 DPI 感知"。如果不是 "每显示器 DPI 感知"，那么很可能就是问题所在。

2.  **测试不同 DPI 设置：**
    在 Windows 显示设置中，尝试更改显示器的缩放比例（例如，从 100% 更改为 150% 或 200%），并观察应用程序的行为。然后尝试在不同 DPI 的显示器之间拖动应用程序窗口。

3.  **检查 Qt 版本：**
    了解你当前使用的 Qt 版本，因为 DPI 支持在不同版本之间有所改进。

### 总结：

界面异常缩小的主要原因通常是 Qt 应用程序对 Windows 高 DPI 缩放和多显示器环境的感知不足。最有效的解决方案是在应用程序启动时通过 `QGuiApplication::setAttribute(Qt::AA_EnableHighDpiScaling);` 和 `QGuiApplication::setHighDpiScaleFactorRoundingPolicy(Qt::HighDpiScaleFactorRoundingPolicy::PassThrough);` 启用高 DPI 支持，并使用 `qt.conf` 文件确保应用程序以 "每显示器 DPI 感知" 模式运行。同时，遵循良好的 Qt 界面布局实践（使用布局管理器、避免硬编码尺寸）也是至关重要的。