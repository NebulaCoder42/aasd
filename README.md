-- Internal Script Hub v1.5
-- By [Your Name]

-- Services
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

-- Variables
local player = Players.LocalPlayer
local mouse = player:GetMouse()
local gui = Instance.new("ScreenGui")
local mainFrame = Instance.new("Frame")
local tabButtons = Instance.new("Frame")
local tabContent = Instance.new("Frame")
local configFrame = Instance.new("Frame")
local consoleFrame = Instance.new("Frame")
local currentTab = "Scripts"
local themes = {
    Dark = {
        Background = Color3.fromRGB(30, 30, 30),
        Foreground = Color3.fromRGB(200, 200, 200),
        Accent = Color3.fromRGB(0, 120, 215)
    },
    Light = {
        Background = Color3.fromRGB(240, 240, 240),
        Foreground = Color3.fromRGB(50, 50, 50),
        Accent = Color3.fromRGB(0, 90, 180)
    },
    Purple = {
        Background = Color3.fromRGB(40, 30, 50),
        Foreground = Color3.fromRGB(220, 200, 240),
        Accent = Color3.fromRGB(150, 80, 220)
    }
}
local currentTheme = "Dark"

-- GUI Creation
gui.Name = "InternalScriptHub"
gui.Parent = game:GetService("CoreGui")

mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 600, 0, 400)
mainFrame.Position = UDim2.new(0.5, -300, 0.5, -200)
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.BackgroundColor3 = themes[currentTheme].Background
mainFrame.BorderSizePixel = 0
mainFrame.ClipsDescendants = true
mainFrame.Parent = gui

local titleBar = Instance.new("Frame")
titleBar.Name = "TitleBar"
titleBar.Size = UDim2.new(1, 0, 0, 30)
titleBar.BackgroundColor3 = themes[currentTheme].Accent
titleBar.BorderSizePixel = 0
titleBar.Parent = mainFrame

local titleText = Instance.new("TextLabel")
titleText.Name = "TitleText"
titleText.Size = UDim2.new(1, -60, 1, 0)
titleText.Position = UDim2.new(0, 10, 0, 0)
titleText.BackgroundTransparency = 1
titleText.Text = "Internal Script Hub"
titleText.TextColor3 = Color3.new(1, 1, 1)
titleText.TextXAlignment = Enum.TextXAlignment.Left
titleText.Font = Enum.Font.GothamBold
titleText.TextSize = 14
titleText.Parent = titleBar

local closeButton = Instance.new("TextButton")
closeButton.Name = "CloseButton"
closeButton.Size = UDim2.new(0, 30, 1, 0)
closeButton.Position = UDim2.new(1, -30, 0, 0)
closeButton.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
closeButton.BorderSizePixel = 0
closeButton.Text = "X"
closeButton.TextColor3 = Color3.new(1, 1, 1)
closeButton.Font = Enum.Font.GothamBold
closeButton.TextSize = 14
closeButton.Parent = titleBar

closeButton.MouseButton1Click:Connect(function()
    gui:Destroy()
end)

-- Make window draggable
local dragging
local dragInput
local dragStart
local startPos

local function update(input)
    local delta = input.Position - dragStart
    mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

titleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
        
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

titleBar.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        update(input)
    end
end)

-- Tabs
tabButtons.Name = "TabButtons"
tabButtons.Size = UDim2.new(1, 0, 0, 40)
tabButtons.Position = UDim2.new(0, 0, 0, 30)
tabButtons.BackgroundColor3 = themes[currentTheme].Background
tabButtons.BorderSizePixel = 0
tabButtons.Parent = mainFrame

tabContent.Name = "TabContent"
tabContent.Size = UDim2.new(1, 0, 1, -70)
tabContent.Position = UDim2.new(0, 0, 0, 70)
tabContent.BackgroundColor3 = themes[currentTheme].Background
tabContent.BorderSizePixel = 0
tabContent.Parent = mainFrame

local function createTabButton(name)
    local button = Instance.new("TextButton")
    button.Name = name .. "Button"
    button.Size = UDim2.new(0.2, 0, 1, 0)
    button.Position = UDim2.new(0.2 * ({"Scripts", "Config", "Console"})[name], 0, 0, 0)
    button.BackgroundColor3 = themes[currentTheme].Background
    button.BorderSizePixel = 0
    button.Text = name
    button.TextColor3 = themes[currentTheme].Foreground
    button.Font = Enum.Font.Gotham
    button.TextSize = 14
    button.Parent = tabButtons
    
    button.MouseButton1Click:Connect(function()
        currentTab = name
        updateTabs()
    end)
    
    return button
end

local scriptsTabButton = createTabButton("Scripts")
local configTabButton = createTabButton("Config")
local consoleTabButton = createTabButton("Console")

-- Scripts Tab
local scriptsScroll = Instance.new("ScrollingFrame")
scriptsScroll.Name = "ScriptsScroll"
scriptsScroll.Size = UDim2.new(1, 0, 1, 0)
scriptsScroll.BackgroundTransparency = 1
scriptsScroll.ScrollBarThickness = 5
scriptsScroll.CanvasSize = UDim2.new(0, 0, 0, 500)
scriptsScroll.Parent = tabContent

local scriptsListLayout = Instance.new("UIListLayout")
scriptsListLayout.Padding = UDim1.new(0, 5)
scriptsListLayout.Parent = scriptsScroll

-- Add scripts
local scripts = {
    {
        Name = "Universal Script",
        Description = "A versatile script with multiple features",
        Execute = function()
            loadstring(game:HttpGet("https://raw.githubusercontent.com/example/universal/main/script.lua"))()
        end
    },
    {
        Name = "Infinite Yield",
        Description = "Admin commands for Roblox",
        Execute = function()
            loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"))()
        end
    },
    {
        Name = "Simple Spy",
        Description = "Remote spy for debugging",
        Execute = function()
            loadstring(game:HttpGet("https://raw.githubusercontent.com/78n/SimpleSpy/main/SimpleSpySource.lua"))()
        end
    },
    {
        Name = "Sunc",
        Description = "Another useful script",
        Execute = function()
            loadstring(game:HttpGet("https://raw.githubusercontent.com/example/sunc/main/script.lua"))()
        end
    }
}

for i, scriptData in ipairs(scripts) do
    local scriptFrame = Instance.new("Frame")
    scriptFrame.Name = scriptData.Name
    scriptFrame.Size = UDim2.new(1, -10, 0, 80)
    scriptFrame.Position = UDim2.new(0, 5, 0, (i-1)*85)
    scriptFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    scriptFrame.BorderSizePixel = 0
    scriptFrame.Parent = scriptsScroll
    
    local scriptName = Instance.new("TextLabel")
    scriptName.Name = "Name"
    scriptName.Size = UDim2.new(1, 0, 0, 30)
    scriptName.Position = UDim2.new(0, 5, 0, 5)
    scriptName.BackgroundTransparency = 1
    scriptName.Text = scriptData.Name
    scriptName.TextColor3 = Color3.new(1, 1, 1)
    scriptName.TextXAlignment = Enum.TextXAlignment.Left
    scriptName.Font = Enum.Font.GothamBold
    scriptName.TextSize = 16
    scriptName.Parent = scriptFrame
    
    local scriptDesc = Instance.new("TextLabel")
    scriptDesc.Name = "Description"
    scriptDesc.Size = UDim2.new(1, -10, 0, 30)
    scriptDesc.Position = UDim2.new(0, 5, 0, 35)
    scriptDesc.BackgroundTransparency = 1
    scriptDesc.Text = scriptData.Description
    scriptDesc.TextColor3 = Color3.fromRGB(200, 200, 200)
    scriptDesc.TextXAlignment = Enum.TextXAlignment.Left
    scriptDesc.Font = Enum.Font.Gotham
    scriptDesc.TextSize = 12
    scriptDesc.Parent = scriptFrame
    
    local executeButton = Instance.new("TextButton")
    executeButton.Name = "ExecuteButton"
    executeButton.Size = UDim2.new(0, 100, 0, 25)
    executeButton.Position = UDim2.new(1, -105, 1, -30)
    executeButton.BackgroundColor3 = themes[currentTheme].Accent
    executeButton.BorderSizePixel = 0
    executeButton.Text = "Execute"
    executeButton.TextColor3 = Color3.new(1, 1, 1)
    executeButton.Font = Enum.Font.Gotham
    executeButton.TextSize = 14
    executeButton.Parent = scriptFrame
    
    executeButton.MouseButton1Click:Connect(function()
        pcall(scriptData.Execute)
    end)
    
    scriptsScroll.CanvasSize = UDim2.new(0, 0, 0, i*85)
end

-- Config Tab
configFrame.Name = "ConfigFrame"
configFrame.Size = UDim2.new(1, 0, 1, 0)
configFrame.BackgroundTransparency = 1
configFrame.Visible = false
configFrame.Parent = tabContent

local configScroll = Instance.new("ScrollingFrame")
configScroll.Name = "ConfigScroll"
configScroll.Size = UDim2.new(1, 0, 1, 0)
configScroll.BackgroundTransparency = 1
configScroll.ScrollBarThickness = 5
configScroll.CanvasSize = UDim2.new(0, 0, 0, 300)
configScroll.Parent = configFrame

local configListLayout = Instance.new("UIListLayout")
configListLayout.Padding = UDim1.new(0, 5)
configListLayout.Parent = configScroll

-- Theme Selector
local themeFrame = Instance.new("Frame")
themeFrame.Name = "ThemeFrame"
themeFrame.Size = UDim2.new(1, -10, 0, 100)
themeFrame.Position = UDim2.new(0, 5, 0, 5)
themeFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
themeFrame.BorderSizePixel = 0
themeFrame.Parent = configScroll

local themeTitle = Instance.new("TextLabel")
themeTitle.Name = "ThemeTitle"
themeTitle.Size = UDim2.new(1, 0, 0, 30)
themeTitle.Position = UDim2.new(0, 5, 0, 5)
themeTitle.BackgroundTransparency = 1
themeTitle.Text = "Theme"
themeTitle.TextColor3 = Color3.new(1, 1, 1)
themeTitle.TextXAlignment = Enum.TextXAlignment.Left
themeTitle.Font = Enum.Font.GothamBold
themeTitle.TextSize = 16
themeTitle.Parent = themeFrame

local themeButtons = {}

for i, themeName in ipairs({"Dark", "Light", "Purple"}) do
    local themeButton = Instance.new("TextButton")
    themeButton.Name = themeName .. "ThemeButton"
    themeButton.Size = UDim2.new(0.3, -10, 0, 30)
    themeButton.Position = UDim2.new(0.3 * (i-1) + 0.05, 0, 0, 40)
    themeButton.BackgroundColor3 = themes[themeName].Background
    themeButton.BorderColor3 = themes[themeName].Accent
    themeButton.Text = themeName
    themeButton.TextColor3 = themes[themeName].Foreground
    themeButton.Font = Enum.Font.Gotham
    themeButton.TextSize = 14
    themeButton.Parent = themeFrame
    
    themeButton.MouseButton1Click:Connect(function()
        currentTheme = themeName
        updateTheme()
    end)
    
    table.insert(themeButtons, themeButton)
end

-- Toggle UI Key
local toggleKeyFrame = Instance.new("Frame")
toggleKeyFrame.Name = "ToggleKeyFrame"
toggleKeyFrame.Size = UDim2.new(1, -10, 0, 80)
toggleKeyFrame.Position = UDim2.new(0, 5, 0, 110)
toggleKeyFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
toggleKeyFrame.BorderSizePixel = 0
toggleKeyFrame.Parent = configScroll

local toggleKeyTitle = Instance.new("TextLabel")
toggleKeyTitle.Name = "ToggleKeyTitle"
toggleKeyTitle.Size = UDim2.new(1, 0, 0, 30)
toggleKeyTitle.Position = UDim2.new(0, 5, 0, 5)
toggleKeyTitle.BackgroundTransparency = 1
toggleKeyTitle.Text = "Toggle UI Key"
toggleKeyTitle.TextColor3 = Color3.new(1, 1, 1)
toggleKeyTitle.TextXAlignment = Enum.TextXAlignment.Left
toggleKeyTitle.Font = Enum.Font.GothamBold
toggleKeyTitle.TextSize = 16
toggleKeyTitle.Parent = toggleKeyFrame

local currentKeyLabel = Instance.new("TextLabel")
currentKeyLabel.Name = "CurrentKeyLabel"
currentKeyLabel.Size = UDim2.new(1, -10, 0, 20)
currentKeyLabel.Position = UDim2.new(0, 5, 0, 35)
currentKeyLabel.BackgroundTransparency = 1
currentKeyLabel.Text = "Current: RightShift"
currentKeyLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
currentKeyLabel.TextXAlignment = Enum.TextXAlignment.Left
currentKeyLabel.Font = Enum.Font.Gotham
currentKeyLabel.TextSize = 12
currentKeyLabel.Parent = toggleKeyFrame

local changeKeyButton = Instance.new("TextButton")
changeKeyButton.Name = "ChangeKeyButton"
changeKeyButton.Size = UDim2.new(0, 120, 0, 25)
changeKeyButton.Position = UDim2.new(0, 5, 0, 55)
changeKeyButton.BackgroundColor3 = themes[currentTheme].Accent
changeKeyButton.BorderSizePixel = 0
changeKeyButton.Text = "Change Key"
changeKeyButton.TextColor3 = Color3.new(1, 1, 1)
changeKeyButton.Font = Enum.Font.Gotham
changeKeyButton.TextSize = 14
changeKeyButton.Parent = toggleKeyFrame

local toggleKey = Enum.KeyCode.RightShift
local listeningForKey = false

changeKeyButton.MouseButton1Click:Connect(function()
    listeningForKey = true
    changeKeyButton.Text = "Press any key..."
    changeKeyButton.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
end)

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if listeningForKey and not gameProcessed then
        if input.UserInputType == Enum.UserInputType.Keyboard then
            toggleKey = input.KeyCode
            currentKeyLabel.Text = "Current: " .. tostring(input.KeyCode):gsub("Enum.KeyCode.", "")
            listeningForKey = false
            changeKeyButton.Text = "Change Key"
            changeKeyButton.BackgroundColor3 = themes[currentTheme].Accent
        end
    elseif input.KeyCode == toggleKey and not gameProcessed then
        mainFrame.Visible = not mainFrame.Visible
    end
end)

-- Console Tab
consoleFrame.Name = "ConsoleFrame"
consoleFrame.Size = UDim2.new(1, 0, 1, 0)
consoleFrame.BackgroundTransparency = 1
consoleFrame.Visible = false
consoleFrame.Parent = tabContent

local consoleOutput = Instance.new("ScrollingFrame")
consoleOutput.Name = "ConsoleOutput"
consoleOutput.Size = UDim2.new(1, -10, 0.7, -10)
consoleOutput.Position = UDim2.new(0, 5, 0, 5)
consoleOutput.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
consoleOutput.ScrollBarThickness = 5
consoleOutput.CanvasSize = UDim2.new(0, 0, 2, 0)
consoleOutput.AutomaticCanvasSize = Enum.AutomaticSize.Y
consoleOutput.Parent = consoleFrame

local consoleOutputLayout = Instance.new("UIListLayout")
consoleOutputLayout.Padding = UDim1.new(0, 5)
consoleOutputLayout.Parent = consoleOutput

local consoleInput = Instance.new("TextBox")
consoleInput.Name = "ConsoleInput"
consoleInput.Size = UDim2.new(1, -10, 0.15, -10)
consoleInput.Position = UDim2.new(0, 5, 0.7, 5)
consoleInput.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
consoleInput.BorderSizePixel = 0
consoleInput.TextColor3 = Color3.new(1, 1, 1)
consoleInput.Font = Enum.Font.Code
consoleInput.TextSize = 14
consoleInput.TextXAlignment = Enum.TextXAlignment.Left
consoleInput.TextYAlignment = Enum.TextYAlignment.Top
consoleInput.MultiLine = true
consoleInput.Text = "-- Enter Lua code here"
consoleInput.Parent = consoleFrame

local executeConsoleButton = Instance.new("TextButton")
executeConsoleButton.Name = "ExecuteConsoleButton"
executeConsoleButton.Size = UDim2.new(0.3, 0, 0.1, -10)
executeConsoleButton.Position = UDim2.new(0.35, 0, 0.85, 5)
executeConsoleButton.BackgroundColor3 = themes[currentTheme].Accent
executeConsoleButton.BorderSizePixel = 0
executeConsoleButton.Text = "Execute"
executeConsoleButton.TextColor3 = Color3.new(1, 1, 1)
executeConsoleButton.Font = Enum.Font.GothamBold
executeConsoleButton.TextSize = 14
executeConsoleButton.Parent = consoleFrame

local clearConsoleButton = Instance.new("TextButton")
clearConsoleButton.Name = "ClearConsoleButton"
clearConsoleButton.Size = UDim2.new(0.3, 0, 0.1, -10)
clearConsoleButton.Position = UDim2.new(0.675, 0, 0.85, 5)
clearConsoleButton.BackgroundColor3 = Color3.fromRGB(100, 30, 30)
clearConsoleButton.BorderSizePixel = 0
clearConsoleButton.Text = "Clear"
clearConsoleButton.TextColor3 = Color3.new(1, 1, 1)
clearConsoleButton.Font = Enum.Font.GothamBold
clearConsoleButton.TextSize = 14
clearConsoleButton.Parent = consoleFrame

local attachWarning = Instance.new("TextLabel")
attachWarning.Name = "AttachWarning"
attachWarning.Size = UDim2.new(1, -10, 0.1, -10)
attachWarning.Position = UDim2.new(0, 5, 0.85, 5)
attachWarning.BackgroundTransparency = 1
attachWarning.Text = "WARNING: Do not use 'attach' functions here!"
attachWarning.TextColor3 = Color3.fromRGB(255, 80, 80)
attachWarning.Font = Enum.Font.GothamBold
attachWarning.TextSize = 12
attachWarning.TextXAlignment = Enum.TextXAlignment.Left
attachWarning.Visible = false
attachWarning.Parent = consoleFrame

consoleInput:GetPropertyChangedSignal("Text"):Connect(function()
    if string.find(consoleInput.Text:lower(), "attach") then
        attachWarning.Visible = true
    else
        attachWarning.Visible = false
    end
end)

executeConsoleButton.MouseButton1Click:Connect(function()
    local success, result = pcall(function()
        return loadstring(consoleInput.Text)()
    end)
    
    local outputText = Instance.new("TextLabel")
    outputText.Name = "ConsoleMessage"
    outputText.Size = UDim2.new(1, -10, 0, 20)
    outputText.BackgroundTransparency = 1
    outputText.TextXAlignment = Enum.TextXAlignment.Left
    outputText.Font = Enum.Font.Code
    outputText.TextSize = 14
    
    if success then
        outputText.Text = "> " .. tostring(result)
        outputText.TextColor3 = Color3.fromRGB(200, 200, 200)
    else
        outputText.Text = "Error: " .. result
        outputText.TextColor3 = Color3.fromRGB(255, 100, 100)
    end
    
    outputText.Parent = consoleOutput
end)

clearConsoleButton.MouseButton1Click:Connect(function()
    for _, child in ipairs(consoleOutput:GetChildren()) do
        if child:IsA("TextLabel") then
            child:Destroy()
        end
    end
end)

-- Functions
local function updateTabs()
    scriptsScroll.Visible = currentTab == "Scripts"
    configFrame.Visible = currentTab == "Config"
    consoleFrame.Visible = currentTab == "Console"
    
    scriptsTabButton.BackgroundColor3 = currentTab == "Scripts" and themes[currentTheme].Accent or themes[currentTheme].Background
    configTabButton.BackgroundColor3 = currentTab == "Config" and themes[currentTheme].Accent or themes[currentTheme].Background
    consoleTabButton.BackgroundColor3 = currentTab == "Console" and themes[currentTheme].Accent or themes[currentTheme].Background
end

local function updateTheme()
    mainFrame.BackgroundColor3 = themes[currentTheme].Background
    titleBar.BackgroundColor3 = themes[currentTheme].Accent
    tabButtons.BackgroundColor3 = themes[currentTheme].Background
    tabContent.BackgroundColor3 = themes[currentTheme].Background
    
    scriptsTabButton.BackgroundColor3 = currentTab == "Scripts" and themes[currentTheme].Accent or themes[currentTheme].Background
    configTabButton.BackgroundColor3 = currentTab == "Config" and themes[currentTheme].Accent or themes[currentTheme].Background
    consoleTabButton.BackgroundColor3 = currentTab == "Console" and themes[currentTheme].Accent or themes[currentTheme].Background
    
    scriptsTabButton.TextColor3 = themes[currentTheme].Foreground
    configTabButton.TextColor3 = themes[currentTheme].Foreground
    consoleTabButton.TextColor3 = themes[currentTheme].Foreground
    
    for _, button in ipairs(themeButtons) do
        button.BackgroundColor3 = themes[button.Text].Background
        button.TextColor3 = themes[button.Text].Foreground
    end
    
    changeKeyButton.BackgroundColor3 = themes[currentTheme].Accent
    executeConsoleButton.BackgroundColor3 = themes[currentTheme].Accent
end

-- Initialize
updateTabs()
updateTheme()

-- Anti-delete
if syn and syn.protect_gui then
    syn.protect_gui(gui)
end
