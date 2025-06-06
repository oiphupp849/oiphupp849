--[[
RGB Hub - Full Mega Script by MasterOfScript
Features:
- Auto Win (super fast)
- Auto Rebirth (super fast)
- Free Gift 1-6 (super fast, stops instantly)
- TP Tabs with buttons for world 2 & 3
- Draggable GUI
- Minimize & Close with confirmation
- Notifications & Loading screen
]]

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local TeleportService = game:GetService("TeleportService")

local player = Players.LocalPlayer
local mouse = player:GetMouse()

-- Create ScreenGui
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "RGBHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = player:WaitForChild("PlayerGui")

-- Main Frame
local Gui = Instance.new("Frame")
Gui.Name = "MainFrame"
Gui.Size = UDim2.new(0, 500, 0, 350)
Gui.Position = UDim2.new(0.5, -250, 0.5, -175)
Gui.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Gui.BorderSizePixel = 0
Gui.Active = true
Gui.Draggable = true
Gui.Parent = ScreenGui

-- Top Bar
local TopBar = Instance.new("Frame")
TopBar.Size = UDim2.new(1, 0, 0, 35)
TopBar.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
TopBar.BorderSizePixel = 0
TopBar.Parent = Gui

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(0.6, 0, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "RGB Hub"
Title.Font = Enum.Font.GothamBold
Title.TextSize = 20
Title.TextColor3 = Color3.fromRGB(0, 255, 100)
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = TopBar

local BetaLabel = Instance.new("TextLabel")
BetaLabel.Size = UDim2.new(0, 60, 1, 0)
BetaLabel.Position = UDim2.new(0.6, 5, 0, 0)
BetaLabel.BackgroundColor3 = Color3.fromRGB(0, 255, 100)
BetaLabel.Text = "BETA"
BetaLabel.Font = Enum.Font.GothamBold
BetaLabel.TextSize = 16
BetaLabel.TextColor3 = Color3.new(0,0,0)
BetaLabel.Parent = TopBar

-- Minimize Button
local MinimizeBtn = Instance.new("TextButton")
MinimizeBtn.Size = UDim2.new(0, 35, 1, 0)
MinimizeBtn.Position = UDim2.new(0.85, 0, 0, 0)
MinimizeBtn.BackgroundColor3 = Color3.fromRGB(35,35,35)
MinimizeBtn.Text = "-"
MinimizeBtn.Font = Enum.Font.GothamBold
MinimizeBtn.TextSize = 24
MinimizeBtn.TextColor3 = Color3.fromRGB(0,255,100)
MinimizeBtn.Parent = TopBar

-- Close Button
local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 35, 1, 0)
CloseBtn.Position = UDim2.new(0.93, 0, 0, 0)
CloseBtn.BackgroundColor3 = Color3.fromRGB(35,35,35)
CloseBtn.Text = "X"
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 20
CloseBtn.TextColor3 = Color3.fromRGB(255, 80, 80)
CloseBtn.Parent = TopBar

-- Tabs container
local TabsFrame = Instance.new("Frame")
TabsFrame.Size = UDim2.new(1, 0, 1, -35)
TabsFrame.Position = UDim2.new(0, 0, 0, 35)
TabsFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
TabsFrame.BorderSizePixel = 0
TabsFrame.Parent = Gui

-- Tabs buttons frame
local TabButtons = Instance.new("Frame")
TabButtons.Size = UDim2.new(0, 120, 1, 0)
TabButtons.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
TabButtons.BorderSizePixel = 0
TabButtons.Parent = TabsFrame

-- Tab buttons UIListLayout
local TabButtonLayout = Instance.new("UIListLayout")
TabButtonLayout.FillDirection = Enum.FillDirection.Vertical
TabButtonLayout.SortOrder = Enum.SortOrder.LayoutOrder
TabButtonLayout.Padding = UDim.new(0, 5)
TabButtonLayout.Parent = TabButtons

-- Content area for tabs
local TabContent = Instance.new("Frame")
TabContent.Size = UDim2.new(1, -120, 1, 0)
TabContent.Position = UDim2.new(0, 120, 0, 0)
TabContent.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
TabContent.BorderSizePixel = 0
TabContent.Parent = TabsFrame

-- Helper function to create tab button
local function CreateTabButton(name)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -10, 0, 35)
    btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    btn.Text = name
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 18
    btn.TextColor3 = Color3.fromRGB(0, 255, 100)
    btn.BorderSizePixel = 0
    btn.Parent = TabButtons
    return btn
end

-- Helper function to create toggle
local function CreateToggle(name, parent, callback)
    local container = Instance.new("Frame")
    container.Size = UDim2.new(1, -20, 0, 30)
    container.BackgroundTransparency = 1
    container.Parent = parent

    local label = Instance.new("TextLabel")
    label.Text = name
    label.Font = Enum.Font.Gotham
    label.TextSize = 16
    label.TextColor3 = Color3.fromRGB(200, 200, 200)
    label.BackgroundTransparency = 1
    label.Size = UDim2.new(0.75, 0, 1, 0)
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = container

    local toggle = Instance.new("TextButton")
    toggle.Size = UDim2.new(0, 50, 0, 20)
    toggle.Position = UDim2.new(0.8, 0, 0.15, 0)
    toggle.BackgroundColor3 = Color3.fromRGB(40,40,40)
    toggle.TextColor3 = Color3.fromRGB(0,255,100)
    toggle.Font = Enum.Font.GothamBold
    toggle.TextSize = 16
    toggle.Text = "OFF"
    toggle.Parent = container

    local enabled = false

    toggle.MouseButton1Click:Connect(function()
        enabled = not enabled
        toggle.Text = enabled and "ON" or "OFF"
        toggle.BackgroundColor3 = enabled and Color3.fromRGB(0, 150, 0) or Color3.fromRGB(40,40,40)
        callback(enabled)
    end)

    return container
end

-- Helper function to create button
local function CreateButton(name, parent, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -20, 0, 30)
    btn.BackgroundColor3 = Color3.fromRGB(40,40,40)
    btn.TextColor3 = Color3.fromRGB(0,255,100)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 16
    btn.Text = name
    btn.BorderSizePixel = 0
    btn.Parent = parent

    btn.MouseButton1Click:Connect(function()
        callback()
    end)

    return btn
end

-- Clear frame helper
local function ClearFrame(frame)
    for _, child in pairs(frame:GetChildren()) do
        if not (child:IsA("UIListLayout") or child:IsA("UIPadding")) then
            child:Destroy()
        end
    end
end

-- Variables for toggles state
local AutoWinEnabled = false
local AutoRebirthEnabled = false
local FreeGiftEnabled = false

-- Auto Win loop
spawn(function()
    while true do
        if AutoWinEnabled then
            ReplicatedStorage:WaitForChild("Events"):WaitForChild("Win"):FireServer()
            RunService.Heartbeat:Wait()
        else
            RunService.Heartbeat:Wait()
        end
    end
end)

-- Auto Rebirth loop
spawn(function()
    while true do
        if AutoRebirthEnabled then
            ReplicatedStorage:WaitForChild("Events"):WaitForChild("Rebirth"):FireServer()
            RunService.Heartbeat:Wait()
        else
            RunService.Heartbeat:Wait()
        end
    end
end)

-- Free Gift loop
spawn(function()
    while true do
        if FreeGiftEnabled then
            for i = 1, 6 do
                if not FreeGiftEnabled then break end
                local args = {i}
                ReplicatedStorage:WaitForChild("Events"):WaitForChild("Reward"):FireServer(unpack(args))
                RunService.Heartbeat:Wait()
            end
            -- Short delay after one full gift cycle
            for _ = 1, 20 do
                if not FreeGiftEnabled then break end
                RunService.Heartbeat:Wait()
            end
        else
            RunService.Heartbeat:Wait()
        end
    end
end)

-- Tabs creation and switching
local Tabs = {}

local function ShowTab(tabName)
    ClearFrame(TabContent)
    local tab = Tabs[tabName]
    if tab then
        for _, element in pairs(tab) do
            element.Parent = TabContent
        end
    end
end

-- Create Main tab
Tabs["Main"] = {}

table.insert(Tabs["Main"], CreateToggle("Auto Win", TabContent, function(state) AutoWinEnabled = state end))
table.insert(Tabs["Main"], CreateToggle("Auto Rebirth", TabContent, function(state) AutoRebirthEnabled = state end))
table.insert(Tabs["Main"], CreateToggle("Free Gift 1 - 6 (Need wait to gift rewards)", TabContent, function(state) FreeGiftEnabled = state end))

-- Create Teleport tab
Tabs["Teleport"] = {}

local tpToWorld2 = CreateButton("TP to World 2", TabContent, function()
    TeleportService:Teleport(14694109570, player)
end)
table.insert(Tabs["Teleport"], tpToWorld2)

local tpToWorld3 = CreateButton("TP to World 3", TabContent, function()
    TeleportService:Teleport(16327847587, player)
end)
table.insert(Tabs["Teleport"], tpToWorld3)

-- Create tab buttons
local MainTabBtn = CreateTabButton("Main")
local TeleportTabBtn = CreateTabButton("Teleport")

-- Show default tab on load
ShowTab("Main")

MainTabBtn.MouseButton1Click:Connect(function()
    ShowTab("Main")
    MainTabBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
    TeleportTabBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
end)

TeleportTabBtn.MouseButton1Click:Connect(function()
    ShowTab("Teleport")
    TeleportTabBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
    MainTabBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
end)

-- Set initial tab button color
MainTabBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
TeleportTabBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)

-- Minimize logic
local minimized = false
MinimizeBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    if minimized then
        -- Slice GUI: show only top bar
        Gui.Size = UDim2.new(0, 500, 0, 35)
        TabsFrame.Visible = false
    else
        Gui.Size = UDim2.new(0, 500, 0, 350)
        TabsFrame.Visible = true
    end
end)

-- Close logic with confirmation
CloseBtn.MouseButton1Click:Connect(function()
    local confirmFrame = Instance.new("Frame")
    confirmFrame.Size = UDim2.new(0, 300, 0, 150)
    confirmFrame.Position = UDim2.new(0.5, -150, 0.5, -75)
    confirmFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
    confirmFrame.BorderSizePixel = 0
    confirmFrame.Parent = ScreenGui
    confirmFrame.ZIndex = 10

    local text = Instance.new("TextLabel")
    text.Size = UDim2.new(1, -20, 0.5, 0)
    text.Position = UDim2.new(0, 10, 0, 10)
    text.BackgroundTransparency = 1
    text.Text = "Are you sure you want to remove RGB Hub?"
    text.TextColor3 = Color3.fromRGB(255, 255, 255)
    text.TextWrapped = true
    text.Font = Enum.Font.GothamBold
    text.TextSize = 18
    text.Parent = confirmFrame

    local yesBtn = Instance.new("TextButton")
    yesBtn.Size = UDim2.new(0.4, 0, 0, 40)
    yesBtn.Position = UDim2.new(0.05, 0, 0.65, 0)
    yesBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
    yesBtn.Text = "Yes"
    yesBtn.TextColor3 = Color3.new(1,1,1)
    yesBtn.Font = Enum.Font.GothamBold
    yesBtn.TextSize = 20
    yesBtn.Parent = confirmFrame

    local noBtn = Instance.new("TextButton")
    noBtn.Size = UDim2.new(0.4, 0, 0, 40)
    noBtn.Position = UDim2.new(0.55, 0, 0.65, 0)
    noBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
    noBtn.Text = "No"
    noBtn.TextColor3 = Color3.new(1,1,1)
    noBtn.Font = Enum.Font.GothamBold
    noBtn.TextSize = 20
    noBtn.Parent = confirmFrame

    yesBtn.MouseButton1Click:Connect(function()
        ScreenGui:Destroy()
    end)

    noBtn.MouseButton1Click:Connect(function()
        confirmFrame:Destroy()
    end)
end)

-- Notification function
local function Notify(text)
    local notifyGui = Instance.new("ScreenGui")
    notifyGui.Name = "NotifyGui"
    notifyGui.Parent = player:WaitForChild("PlayerGui")

    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 250, 0, 50)
    frame.Position = UDim2.new(0.5, -125, 0, 10)
    frame.BackgroundColor3 = Color3.fromRGB(0, 255, 100)
    frame.BackgroundTransparency = 0.1
    frame.Parent = notifyGui

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -10, 1, -10)
    label.Position = UDim2.new(0, 5, 0, 5)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Color3.fromRGB(0,0,0)
    label.Font = Enum.Font.GothamBold
    label.TextSize = 18
    label.Parent = frame

    delay(3, function()
        notifyGui:Destroy()
    end)
end

-- Show loading screen
local loadingScreen = Instance.new("Frame")
loadingScreen.Size = UDim2.new(1,0,1,0)
loadingScreen.BackgroundColor3 = Color3.fromRGB(0,0,0)
loadingScreen.Parent = ScreenGui
loadingScreen.ZIndex = 20

local loadingText = Instance.new("TextLabel")
loadingText.Size = UDim2.new(1, 0, 0, 50)
loadingText.Position = UDim2.new(0, 0, 0.5, -25)
loadingText.BackgroundTransparency = 1
loadingText.TextColor3 = Color3.fromRGB(0,255,100)
loadingText.Font = Enum.Font.GothamBold
loadingText.TextSize = 24
loadingText.Text = "Loading the GUI...\nScript by MasterOfScript...\nLike and Subscribe!"
loadingText.TextWrapped = true
loadingText.Parent = loadingScreen

wait(2)
loadingScreen:Destroy()
Notify("Script By MasterOfScript")

-- That's it, your MEGA SUPER SAIYAN RGB Hub script is ready.
