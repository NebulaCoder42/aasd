-- Gui Creation
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "InternalScriptHub"

-- Dependencies
local UIS = game:GetService("UserInputService")
local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- Main Frame
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 600, 0, 400)
MainFrame.Position = UDim2.new(0.5, -300, 0.5, -200)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true

-- Tabs
local Tabs = Instance.new("Frame", MainFrame)
Tabs.Size = UDim2.new(0, 120, 1, 0)
Tabs.BackgroundColor3 = Color3.fromRGB(25, 25, 25)

local function createTab(name)
	local btn = Instance.new("TextButton", Tabs)
	btn.Size = UDim2.new(1, 0, 0, 40)
	btn.Text = name
	btn.TextColor3 = Color3.new(1,1,1)
	btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	btn.BorderSizePixel = 0
	btn.Font = Enum.Font.SourceSansBold
	btn.TextSize = 14
	btn.AutoButtonColor = true
	return btn
end

-- Tab Buttons
local ScriptHubBtn = createTab("Script Hub")
local ExecutorBtn = createTab("Executor")
local ConsoleBtn = createTab("Console")
local SettingsBtn = createTab("Settings")

-- Content Frame
local ContentFrame = Instance.new("Frame", MainFrame)
ContentFrame.Position = UDim2.new(0, 120, 0, 0)
ContentFrame.Size = UDim2.new(1, -120, 1, 0)
ContentFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)

local function clearContent()
	for _, v in pairs(ContentFrame:GetChildren()) do
		if v:IsA("GuiObject") then v:Destroy() end
	end
end

----------------------
-- SCRIPT HUB PANEL --
----------------------
local function loadScriptHub()
	clearContent()
	local label = Instance.new("TextLabel", ContentFrame)
	label.Size = UDim2.new(1, 0, 0, 40)
	label.Text = "Script Hub"
	label.Font = Enum.Font.SourceSansBold
	label.TextSize = 18
	label.TextColor3 = Color3.new(1, 1, 1)
	label.BackgroundTransparency = 1

	local scripts = {
		{ Name = "Infinite Yield", URL = "https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source" },
		{ Name = "Unnamed ESP", URL = "https://raw.githubusercontent.com/ic3w0lf22/Unnamed-ESP/master/UnnamedESP.lua" },
		{ Name = "Simple Spy", URL = "https://raw.githubusercontent.com/exxtremestuffs/SimpleSpySource/master/SimpleSpy.lua" },
		{ Name = "SUNC", URL = "https://raw.githubusercontent.com/SussySonic/SUNC/main/main.lua" }
	}

	for i, script in ipairs(scripts) do
		local btn = Instance.new("TextButton", ContentFrame)
		btn.Size = UDim2.new(1, -20, 0, 40)
		btn.Position = UDim2.new(0, 10, 0, 40 + (i - 1) * 45)
		btn.Text = script.Name
		btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
		btn.TextColor3 = Color3.new(1,1,1)
		btn.Font = Enum.Font.SourceSans
		btn.TextSize = 14
		btn.MouseButton1Click:Connect(function()
			loadstring(game:HttpGet(script.URL))()
		end)
	end
end

----------------------
-- EXECUTOR PANEL ---
----------------------
local function loadExecutor()
	clearContent()

	local textBox = Instance.new("TextBox", ContentFrame)
	textBox.Size = UDim2.new(1, -20, 0.8, -20)
	textBox.Position = UDim2.new(0, 10, 0, 10)
	textBox.TextXAlignment = Enum.TextXAlignment.Left
	textBox.TextYAlignment = Enum.TextYAlignment.Top
	textBox.TextWrapped = true
	textBox.ClearTextOnFocus = false
	textBox.TextEditable = true
	textBox.MultiLine = true
	textBox.Font = Enum.Font.Code
	textBox.TextSize = 14
	textBox.TextColor3 = Color3.new(1,1,1)
	textBox.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
	textBox.Text = "-- Escribe tu script Lua aquí"

	local execBtn = Instance.new("TextButton", ContentFrame)
	execBtn.Position = UDim2.new(0, 10, 0.85, 0)
	execBtn.Size = UDim2.new(0, 100, 0, 40)
	execBtn.Text = "Ejecutar"
	execBtn.BackgroundColor3 = Color3.fromRGB(40, 100, 40)
	execBtn.TextColor3 = Color3.new(1,1,1)
	execBtn.MouseButton1Click:Connect(function()
		loadstring(textBox.Text)()
	end)
end

-------------------
-- CONSOLE PANEL --
-------------------
local function loadConsole()
	clearContent()

	local consoleBox = Instance.new("TextBox", ContentFrame)
	consoleBox.Size = UDim2.new(1, -20, 1, -20)
	consoleBox.Position = UDim2.new(0, 10, 0, 10)
	consoleBox.TextEditable = false
	consoleBox.MultiLine = true
	consoleBox.Font = Enum.Font.Code
	consoleBox.TextSize = 14
	consoleBox.TextXAlignment = Enum.TextXAlignment.Left
	consoleBox.TextYAlignment = Enum.TextYAlignment.Top
	consoleBox.TextColor3 = Color3.new(0,1,0)
	consoleBox.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
	consoleBox.Text = "[Console]: Esperando logs...\n"

	local function log(text)
		consoleBox.Text = consoleBox.Text .. "\n" .. text
	end

	log("Script cargado correctamente.")
end

--------------------
-- SETTINGS PANEL --
--------------------
local function loadSettings()
	clearContent()

	local themeToggle = Instance.new("TextButton", ContentFrame)
	themeToggle.Size = UDim2.new(0, 200, 0, 40)
	themeToggle.Position = UDim2.new(0, 10, 0, 10)
	themeToggle.Text = "Cambiar Tema"
	themeToggle.BackgroundColor3 = Color3.fromRGB(50, 50, 100)
	themeToggle.TextColor3 = Color3.new(1,1,1)

	local isDark = true
	themeToggle.MouseButton1Click:Connect(function()
		isDark = not isDark
		if isDark then
			MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
		else
			MainFrame.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
		end
	end)
end

-- Button Clicks
ScriptHubBtn.MouseButton1Click:Connect(loadScriptHub)
ExecutorBtn.MouseButton1Click:Connect(loadExecutor)
ConsoleBtn.MouseButton1Click:Connect(loadConsole)
SettingsBtn.MouseButton1Click:Connect(loadSettings)

-- Load default tab
loadScriptHub()
