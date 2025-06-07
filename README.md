-- script is protected by MasterOfScript
-- unauthorized use or edits are prohibited
-- respect the creator: MasterOfScript

--[[






























































































]]

local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/shlexware/Orion/main/source"))()
local Window = OrionLib:MakeWindow({
    Name = "RGB Hub (BETA)",
    HidePremium = false,
    SaveConfig = true,
    ConfigFolder = "RGBHubMobile"
})

-- Loading screen
OrionLib:MakeNotification({
    Name = "Loading...",
    Content = "Loading the GUI...\nScript by MasterOfScript\nLike and Subscribe!",
    Image = "rbxassetid://4483345998",
    Time = 5
})

-- MAIN TAB
local MainTab = Window:MakeTab({
    Name = "Main",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

-- Auto Win
local autoWin = false
MainTab:AddToggle({
    Name = "Auto Win",
    Default = false,
    Callback = function(value)
        autoWin = value
        task.spawn(function()
            while autoWin do
                game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Win"):FireServer()
                task.wait()
            end
        end)
    end
})

-- Auto Rebirth
local autoRebirth = false
MainTab:AddToggle({
    Name = "Auto Rebirth",
    Default = false,
    Callback = function(value)
        autoRebirth = value
        task.spawn(function()
            while autoRebirth do
                game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Rebirth"):FireServer()
                task.wait()
            end
        end)
    end
})

-- Free Gift 1-6
local autoGift = false
MainTab:AddToggle({
    Name = "Free Gift 1 - 6 (Need wait for reward)",
    Default = false,
    Callback = function(value)
        autoGift = value
        task.spawn(function()
            while autoGift do
                for i = 1, 6 do
                    game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("Reward"):FireServer(i)
                    task.wait()
                end
            end
        end)
    end
})

-- TELEPORT TAB
local TeleportTab = Window:MakeTab({
    Name = "Teleport",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

-- TP to world 2
TeleportTab:AddButton({
    Name = "TP to World 2",
    Callback = function()
        game:GetService("TeleportService"):Teleport(14694109570, game.Players.LocalPlayer)
    end
})

-- TP to world 3
TeleportTab:AddButton({
    Name = "TP to World 3",
    Callback = function()
        game:GetService("TeleportService"):Teleport(16327847587, game.Players.LocalPlayer)
    end
})

-- GUI Close with confirmation
Window:MakeTab({
    Name = "⚙️ Settings",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
}):AddButton({
    Name = "❌ Close GUI",
    Callback = function()
        OrionLib:MakeNotification({
            Name = "Confirm Close",
            Content = "Are you sure you want to remove this GUI?",
            Image = "rbxassetid://4483345998",
            Time = 5
        })
        task.wait(5)
        OrionLib:Destroy()
    end
})

-- FINALIZE UI
OrionLib:Init()
