# MOUSE_MODE_CAPTURED

**MOUSE_MODE_CAPTURED** គឺជាថេរ (constant) មួយក្នុង `Input` class របស់ Godot 4 ដែលប្រើសម្រាប់គ្រប់គ្រងស្ថានភាពកណ្តុរ (mouse)។

## 📋 និយមន័យ និងការប្រើប្រាស់

```gdscript
enum MouseMode:
    MOUSE_MODE_VISIBLE = 0      # កណ្តុរឃើញ និងផ្លាស់ទីបានសេរី
    MOUSE_MODE_HIDDEN = 1       # កណ្តុរលាក់ ប៉ុន្តែផ្លាស់ទីបាន
    MOUSE_MODE_CAPTURED = 2     # ⭐ កណ្តុរលាក់ និងជាប់នៅកណ្តាលអេក្រង់
    MOUSE_MODE_CONFINED = 3     # កណ្តុរឃើញ ប៉ុន្តែមិនអាចចេញពីបង្អួចហ្គេម
    MOUSE_MODE_CONFINED_HIDDEN = 4  # កណ្តុរលាក់ និងមិនអាចចេញពីបង្អួចហ្គេម
```

### 🔹 **MOUSE_MODE_CAPTURED = 2**
- កណ្តុរនឹង **លាក់** មិនឃើញ [[19]]
- ទីតាំងកណ្តុរនឹង **ជាប់នៅកណ្តាល** បង្អួចហ្គេម [[19]]
- ល្អសម្រាប់ហ្គេម 3D ដែលត្រូវការមើលជុំវិញ (mouselook/camera control) [[1]]

> ⚠️ **ចំណាំសំខាន់៖** នៅពេលប្រើ `MOUSE_MODE_CAPTURED` ប្រសិនបើអ្នកចង់ដំណើរការចលនាកណ្តុរ អ្នកត្រូវប្រើ `InputEventMouseMotion.relative` ជំនួសឱ្យ `event.position` ព្រោះ `event.position` នឹងត្រឡប់មកកណ្តាលអេក្រង់ជានិច្ច។ [[2]]

## 💻 ឧទាហរណ៍កូដ

```gdscript
# កំណត់កណ្តុរជាប់នៅពេលហ្គេមចាប់ផ្តើម
func _ready():
    Input.mouse_mode = Input.MOUSE_MODE_CAPTURED

# បើក/បិទ កណ្តុរនៅពេលចុច ESC
func _input(event):
    if event.is_action_pressed("ui_cancel"):  # ESC key
        if Input.mouse_mode == Input.MOUSE_MODE_CAPTURED:
            Input.mouse_mode = Input.MOUSE_MODE_VISIBLE
        else:
            Input.mouse_mode = Input.MOUSE_MODE_CAPTURED

# ដំណើរការចលនាកណ្តុរសម្រាប់ camera look
func _input(event):
    if event is InputEventMouseMotion and Input.mouse_mode == Input.MOUSE_MODE_CAPTURED:
        # ប្រើ .relative ដើម្បីទទួលបានការផ្លាស់ទីពិតប្រាកដ
        var sensitivity = 0.003
        rotation_y -= event.relative.x * sensitivity
        rotation_x -= event.relative.y * sensitivity
        rotation_x = clamp(rotation_x, deg_to_rad(-90), deg_to_rad(90))
```

## 🔗 តំណភ្ជាប់ឯកសារផ្លូវការ (Godot 4 - GitHub)

1. **Class Input - MouseMode Enum** (GitHub raw):  
   👉 https://raw.githubusercontent.com/godotengine/godot-docs/master/classes/class_input.rst  
   *(ស្វែងរក `.. _class_Input_constant_MOUSE_MODE_CAPTURED:`)* [[19]]

2. **Mouse Capture Tutorial - KidsCanCode (Godot 4 Recipes)**:  
   👉 https://kidscancode.org/godot_recipes/4.x/input/mouse_capture/index.html [[1]]

3. **Mouse and Input Coordinates - Official Docs**:  
   👉 https://docs.godotengine.org/en/stable/tutorials/inputs/mouse_and_input_coordinates.html [[2]]

4. **Input Class Reference - Godot 4.4**:  
   👉 https://docs.godotengine.org/en/4.4/classes/class_input.html [[19]]

## 🛠️ គន្លឹះបន្ថែម

| បញ្ហា | ដំណោះស្រាយ |
|--------|-------------|
| កណ្តុរមិនចេញពីហ្គេមនៅពេលចុច ESC | ត្រូវប្តូរទៅ `MOUSE_MODE_VISIBLE` ជាមុនសិន |
| ចង់ដឹងថាកណ្តុរកំពុងជាប់ឬអត់ | ប្រើ `if Input.mouse_mode == Input.MOUSE_MODE_CAPTURED:` |
| ចលនាកណ្តុរមិនរលូន | ប្រើ `event.screen_relative` ជំនួស `event.relative` សម្រាប់ resolution ផ្សេងៗ [[2]] |
| ហ្គេមនៅលើ Web មិនអាច capture កណ្តុរ | ត្រូវចុចលើហ្គេមជាមុនសិន (user gesture) ទើបអាច capture បាន |
