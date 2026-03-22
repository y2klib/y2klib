# Y2KLib
**Pure black minimal Roblox UI Library**

---

## Load

```lua
local Y2KLib = loadstring(game:HttpGet("https://pastebin.com/raw/UBGys3f2"))()
```

---

## Create Window

```lua
local Window = Y2KLib:CreateWindow({
    Title     = "My Hub",
    Game      = "Blox Fruits",
    ToggleKey = Enum.KeyCode.RightShift,
})
```

| Option | Type | Description |
|--------|------|-------------|
| `Title` | string | Window title |
| `Game` | string | Game name shown in topbar |
| `ToggleKey` | Enum.KeyCode | Key to show/hide the window |

---

## Create Tab

```lua
local Tab = Window:CreateTab("Combat", "⚔️")
```

---

## Elements

### Section
```lua
Tab:CreateSection("Aimbot")
```

### Separator
```lua
Tab:CreateSeparator()
```

### Label
```lua
Tab:CreateLabel("This is a label")
```

### Paragraph
```lua
Tab:CreateParagraph({
    Title = "Info",
    Body  = "This is a paragraph with a title and body text.",
})
```

### Toggle
```lua
local MyToggle = Tab:CreateToggle({
    Name     = "Silent Aim",
    Default  = false,
    Callback = function(value)
        print("Toggle:", value)
    end,
})

-- API
MyToggle:Set(true)
MyToggle:Get() -- returns bool
```

### Button
```lua
Tab:CreateButton({
    Name     = "Kill All",
    Callback = function()
        print("Clicked!")
    end,
})
```

### Slider
```lua
local MySlider = Tab:CreateSlider({
    Name     = "Walk Speed",
    Min      = 16,
    Max      = 500,
    Default  = 16,
    Suffix   = "",     -- optional e.g. "px" or "%"
    Callback = function(value)
        print("Slider:", value)
    end,
})

-- API
MySlider:Set(100)
MySlider:Get() -- returns number
```

### Dropdown
```lua
local MyDropdown = Tab:CreateDropdown({
    Name     = "Target Part",
    Options  = {"Head", "Torso", "HumanoidRootPart"},
    Default  = "Head",
    Multi    = false,  -- set true for multi-select
    Callback = function(value)
        print("Selected:", value)
    end,
})

-- API
MyDropdown:Set("Torso")
MyDropdown:Get() -- returns string (or table if Multi = true)
```

### Textbox
```lua
local MyTextbox = Tab:CreateTextbox({
    Name        = "Player Name",
    Placeholder = "Username...",
    Default     = "",
    Numeric     = false, -- true = numbers only
    Live        = false, -- true = fires on every keystroke
    Callback    = function(value, enterPressed)
        print("Input:", value)
    end,
})

-- API
MyTextbox:Set("Hello")
MyTextbox:Get() -- returns string
```

### Keybind
```lua
local MyKeybind = Tab:CreateKeybind({
    Name     = "Toggle Fly",
    Default  = Enum.KeyCode.F,
    Callback = function(key)
        print("Pressed:", key.Name)
    end,
})

-- API
MyKeybind:Set(Enum.KeyCode.G)
MyKeybind:Get() -- returns Enum.KeyCode
```

### ColorPicker
```lua
local MyColor = Tab:CreateColorPicker({
    Name     = "ESP Color",
    Default  = Color3.fromRGB(255, 100, 100),
    Callback = function(color)
        print("Color:", color)
    end,
})

-- API
MyColor:Set(Color3.fromRGB(0, 255, 0))
MyColor:Get() -- returns Color3
```

### Config (Save / Load)
```lua
Tab:CreateConfig({
    Callback = function(data)
        print("Config loaded!")
    end,
})
```

---

## Notifications

```lua
Window:Notify("Title", "Description", "success", 3)
```

| Type | Color |
|------|-------|
| `info` | White |
| `success` | Green |
| `warning` | Orange |
| `error` | Red |

Or as a table:
```lua
Window:Notify({
    Title       = "Done",
    Description = "Action completed.",
    Type        = "success",
    Duration    = 3,
})
```

---

## Full Example

```lua
local Y2KLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/y2klib/y2klib/refs/heads/main/main%20loader"))()

local Window = Y2KLib:CreateWindow({
    Title     = "Y2KLib",
    Game      = "Blox Fruits",
    ToggleKey = Enum.KeyCode.RightShift,
})

local Tab = Window:CreateTab("Combat", "⚔️")

Tab:CreateSection("Aimbot")

Tab:CreateToggle({
    Name     = "Silent Aim",
    Default  = false,
    Callback = function(v)
        _G.SilentAim = v
    end,
})

Tab:CreateSlider({
    Name     = "Walk Speed",
    Min      = 16,
    Max      = 500,
    Default  = 16,
    Callback = function(v)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = v
    end,
})

Tab:CreateDropdown({
    Name     = "Target Part",
    Options  = {"Head", "Torso", "HumanoidRootPart"},
    Default  = "Head",
    Callback = function(v)
        _G.AimPart = v
    end,
})

Tab:CreateButton({
    Name     = "Rejoin",
    Callback = function()
        game:GetService("TeleportService"):Teleport(game.PlaceId)
    end,
})

Window:Notify("Y2KLib", "Loaded!", "success", 3)
```

---

## Keybinds

| Key | Action |
|-----|--------|
| `RightShift` (default) | Toggle window |
| `─` button | Minimize |
| `✕` button | Close + autosave config |

---

*Y2KLib — Pure Black Minimal*
