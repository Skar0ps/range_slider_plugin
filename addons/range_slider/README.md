# RangeSlider

A range slider to edit `Vector2` properties (a min/max range) with two handles, instead of two separate spinboxes.

Full documentation, screenshots and examples: https://github.com/Skar0ps/range_slider_plugin

## Installation

1. Copy the `addons/range_slider` folder into your project's `addons` directory.
2. Go to `Project > Project Settings > Plugins` and enable the "RangeSlider" plugin.

## Usage

There are two ways to use the plugin, and they work independently:
- **In the inspector**, on a `Vector2` property (no scene or node needed).
- **As a node** (`HRangeSlider` / `VRangeSlider`) in your own scene, for game UI.

### Inspector

The standard `@export_range` does not support `Vector2`, so use `@export_custom` with a range hint. The plugin then replaces the default `Vector2` editor with the slider.

```gdscript
# A range from 0 to 200 with a step of 1.
@export_custom(PROPERTY_HINT_RANGE, "0,200,1") var position_range: Vector2 = Vector2(25.0, 75.0)
```

### Node

Add an `HRangeSlider` or `VRangeSlider` to your scene. Set `min_value`, `max_value` and `step` (inherited from `Range`) plus the starting `value_range`. Styles, icons and colors are in the **Visuals** group of the inspector.

```gdscript
@onready var slider: HRangeSlider = $HRangeSlider

func _ready() -> void:
	slider.min_value = 0.0
	slider.max_value = 100.0
	slider.step = 1.0
	slider.start_value = 20 # same as value_range.x
	slider.end_value = 80   # same as value_range.y
	slider.range_changed.connect(_on_range_changed)

func _on_range_changed(new_range: Vector2) -> void:
	print("start = %s, end = %s" % [new_range.x, new_range.y])
```

Useful members:
- `value_range: Vector2` — the range, `x` is the start and `y` is the end. Clamped to `[min_value, max_value]`.
- `start_value` / `end_value` — shortcuts for `value_range.x` / `.y`. (`slider.value_range.x = 10` won't work, it edits a copy; use `start_value`.)
- `range_changed(new_range: Vector2)` — emitted every time the range changes.
- `editable` / `scrollable` / `always_show_handles` — behaviour toggles.

## Hint String Options

The hint string follows `"min,max,step,options"`:

- `min` / `max`: the bounds of the slider.
- `step` (optional): the snapping increment. Defaults to `1.0`.
- `options` (optional), a comma-separated list of keywords:
	- `or_greater`: allow values greater than `max` in the spinbox.
	- `or_less`: allow values less than `min` in the spinbox.
	- `exp`: exponential (logarithmic) scaling, useful for properties like scale.
	- `radians_as_degrees`: treat the underlying `Vector2` (stored in radians) as degrees in the UI. Adds a `°` suffix.
	- `degrees`: adds a `°` suffix. Use it if your value is already in degrees.
	- `hide_slider` / `hide_control`: prevent the plugin from activating, showing the default `Vector2` editor.

## License

MIT License (see LICENSE).
