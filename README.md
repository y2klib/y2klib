# Y2KLib
**A pure black minimal Roblox UI library for script hubs.**

> Clean. Fast. No fluff.

---

## 📦 Load the Library

Paste this at the top of your script:

```lua
local Y2KLib = loadstring(game:HttpGet("YOUR_RAW_URL_HERE"))()
```

Replace `YOUR_RAW_URL_HERE` with the raw link to your hosted `Y2KLib.lua` file (GitHub Raw, Pastefy, etc.)

---

## 🪟 Creating a Window

```lua
local Window = Y2KLib:CreateWindow({
    Title     = "My Hub",       -- Hub title shown in topbar
    Game      = "Blox Fruits",  -- Game name shown in topbar
    ToggleKey = Enum.KeyCode.RightShift, -- Key to show/hide GUI
})
```

| Option | Type | Description |
|---|---|---|
| `Title` | string | Title shown in the topbar |
| `Game` | string | Game name shown in topbar |
| `ToggleKey` | KeyCode | Key to toggle GUI visibility |

---

## 📑 Creating Tabs

```lua
local Tab = Window:CreateTab("Combat", "⚔️")
```

| Argument | Type | Description |
|---|---|---|
| `name` | string | Tab name shown in sidebar |
| `icon` | string | Optional emoji icon shown before name |

---

## 🧩 Elements

All elements are created on a Tab object.

---

### Section
A labeled divider to group elements.
```lua
Tab:CreateSection("Aimbot")
```

---

### Separator
A plain thin divider line.
```lua
Tab:CreateSeparator()
```

---

### Label
A simple line of text.
```lua
Tab:CreateLabel("This is a label.")
```

---

### Paragraph
A block with an optional title and body text.
```lua
Tab:CreateParagraph({
    Title = "About",
    Body  = "This hub was made with Y2KLib.",
})
```

---

### Toggle
An ON/OFF switch.
```lua
local MyToggle = Tab:CreateToggle({
    Name     = "Silent Aim",
    Default  = false,
    Callback = function(value)
        -- value is true or false
        _G.SilentAim = value
    end,
})

-- API
MyToggle:Set(true)   -- force a value
MyToggle:Get()       -- returns current value
```

| Option | Type | Description |
|---|---|---|
| `Name` | string | Element label |
| `Default` | bool | Starting value |
| `Callback` | function | Fires when toggled, passes `true`/`false` |

---

### Button
A clickable button.
```lua
Tab:CreateButton({
    Name     = "Kill All",
    Callback = function()
        -- runs on click
    end,
})
```

---

### Slider
A draggable number slider.
```lua
local MySlider = Tab:CreateSlider({
    Name     = "Walk Speed",
    Min      = 16,
    Max      = 500,
    Default  = 16,
    Suffix   = "",       -- optional unit label e.g. "px" "%" " st"
    Callback = function(value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = value
    end,
})

-- API
MySlider:Set(100)  -- force a value
MySlider:Get()     -- returns current value
```

| Option | Type | Description |
|---|---|---|
| `Min` | number | Minimum value |
| `Max` | number | Maximum value |
| `Default` | number | Starting value |
| `Suffix` | string | Text appended to value display |

---

### Dropdown
A single or multi-select dropdown menu.
```lua
-- Single select
local MyDropdown = Tab:CreateDropdown({
    Name     = "Target Part",
    Options  = {"Head", "Torso", "HumanoidRootPart"},
    Default  = "Head",
    Callback = function(value)
        _G.AimPart = value
    end,
})

-- Multi select
local MyMulti = Tab:CreateDropdown({
    Name     = "Active Modes",
    Options  = {"PvP", "PvE", "Farm", "Boss"},
    Multi    = true,
    Callback = function(selected)
        -- selected is a table: {PvP = true, Farm = true, ...}
    end,
})

-- API
MyDropdown:Set("Torso")
MyDropdown:Get()     -- returns selected value
```

| Option | Type | Description |
|---|---|---|
| `Options` | table | List of choices |
| `Default` | string | Starting selected value |
| `Multi` | bool | Enables multi-select mode |

---

### Textbox
A text input field.
```lua
local MyBox = Tab:CreateTextbox({
    Name        = "Teleport To",
    Placeholder = "Username...",
    Default     = "",
    Numeric     = false,  -- only allow numbers if true
    Live        = false,  -- fires callback on every keystroke if true
    Callback    = function(value, enterPressed)
        if enterPressed then
            print("Entered:", value)
        end
    end,
})

-- API
MyBox:Set("PlayerName")
MyBox:Get()   -- returns current text
```

---

### Keybind
A button that listens for a key press. Click it, then press any key to bind.
```lua
local MyKey = Tab:CreateKeybind({
    Name     = "Toggle Fly",
    Default  = Enum.KeyCode.F,
    Callback = function(key)
        -- fires every time the bound key is pressed
        print("Key pressed:", key.Name)
    end,
})

-- API
MyKey:Set(Enum.KeyCode.G)
MyKey:Get()   -- returns current KeyCode
```

---

### ColorPicker
A hue/saturation/value color picker.
```lua
local MyColor = Tab:CreateColorPicker({
    Name     = "ESP Color",
    Default  = Color3.fromRGB(255, 100, 100),
    Callback = function(color)
        _G.ESPColor = color
    end,
})

-- API
MyColor:Set(Color3.fromRGB(0, 255, 0))
MyColor:Get()   -- returns current Color3
```

---

### Config (Save / Load)
Built-in save and load buttons. Saves to executor filesystem.
```lua
Tab:CreateConfig({
    Callback = function(data)
        -- fires after a config is loaded
        print("Config loaded!")
    end,
})
```

Configs are saved to: `Y2KLib/YourHubTitle/configname.json`

---

## 🔔 Notifications

Send a toast notification from anywhere in your script.

```lua
Window:Notify("Title", "Description", "success", 3)
```

Or using a table:
```lua
Window:Notify({
    Title       = "Teleport",
    Description = "Arrived at destination",
    Type        = "success",   -- "info" | "success" | "error" | "warning"
    Duration    = 3,
})
```

---

## 📋 Full Example

```lua
local Y2KLib = loadstring(game:HttpGet("YOUR_RAW_URL_HERE"))()

local Window = Y2KLib:CreateWindow({
    Title     = "Y2KLib",
    Game      = "Blox Fruits",
    ToggleKey = Enum.KeyCode.RightShift,
})

local Combat = Window:CreateTab("Combat", "⚔️")

Combat:CreateSection("Aimbot")

Combat:CreateToggle({
    Name     = "Silent Aim",
    Default  = false,
    Callback = function(v) _G.SilentAim = v end,
})

Combat:CreateSlider({
    Name     = "Walk Speed",
    Min      = 16,
    Max      = 500,
    Default  = 16,
    Callback = function(v)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = v
    end,
})

Combat:CreateDropdown({
    Name     = "Target Part",
    Options  = {"Head", "Torso", "HumanoidRootPart"},
    Default  = "Head",
    Callback = function(v) _G.AimPart = v end,
})

Combat:CreateButton({
    Name     = "Kill All",
    Callback = function()
        Window:Notify("Kill All", "Executed!", "success", 2)
    end,
})

local Settings = Window:CreateTab("Settings", "⚙️")

Settings:CreateKeybind({
    Name     = "Toggle Fly",
    Default  = Enum.KeyCode.F,
    Callback = function() print("Fly toggled") end,
})

Settings:CreateColorPicker({
    Name     = "ESP Color",
    Default  = Color3.fromRGB(255, 100, 100),
    Callback = function(c) _G.ESPColor = c end,
})

Settings:CreateConfig({})

Window:Notify("Y2KLib", "Loaded successfully!", "success", 3)
```

---

## ⌨️ Default Keybinds

| Key | Action |
|---|---|
| `RightShift` | Toggle GUI visibility (default, can be changed via `ToggleKey`) |

---

## 📁 File Structure

```
Y2KLib/
└── YourHubTitle/
    ├── autosave.json   ← saved on GUI close
    └── default.json    ← saved manually via CreateConfig
```

---

## ✅ Tested Executors
- Delta
- Fluxus
- Synapse X
- Arc

---

## 📄 License
Free to use and modify. Credit appreciated but not required.
