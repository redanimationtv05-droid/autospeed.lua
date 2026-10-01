local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/main/Addons/SaveManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "⚡ SUPER SPEED HUB [PRO VERSION]",
    SubTitle = "by YourName",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 400),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl
})

local Tabs = {
    Main = Window:AddTab({ Title = "Siêu Tốc Độ", Icon = "zap" }),
    Settings = Window:AddTab({ Title = "Cài Đặt", Icon = "settings" })
}

local Options = Fluent.Options

-- Biến điều khiển
local SpeedValue = 16
local SpeedToggle = false
local Connection = nil

-- Hàm Bypass giữ tốc độ liên tục (Bypass Anti-Cheat & Game Reset)
local function ToggleSpeed(state)
    SpeedToggle = state
    if Connection then 
        Connection:Disconnect() 
        Connection = nil 
    end

    if SpeedToggle then
        Connection = game:GetService("RunService").RenderStepped:Connect(function()
            local char = game.Players.LocalPlayer.Character
            if char and char:FindFirstChild("Humanoid") then
                char.Humanoid.WalkSpeed = SpeedValue
            end
        end)
    else
        local char = game.Players.LocalPlayer.Character
        if char and char:FindFirstChild("Humanoid") then
            char.Humanoid.WalkSpeed = 16
        end
    end
end

-- UI Controls
Tabs.Main:AddParagraph({
    Title = "🔥 Chế Độ Siêu Tốc Độ Custom",
    Content = "Chế độ này ép xung WalkSpeed liên tục theo từng frame, giúp giữ tốc độ ổn định ngay cả khi game cố gắng reset."
})

local Toggle = Tabs.Main:AddToggle("SuperSpeedToggle", {
    Title = "Bật Siêu Tốc Độ Pro", 
    Default = false,
    Callback = function(Value)
        ToggleSpeed(Value)
    end
})

local Slider = Tabs.Main:AddSlider("SpeedSlider", {
    Title = "Tốc Độ Di Chuyển (WalkSpeed)",
    Description = "Chỉnh tốc độ tùy ý từ 16 đến 2500",
    Default = 100,
    Min = 16,
    Max = 2500,
    Rounding = 0,
    Callback = function(Value)
        SpeedValue = Value
    end
})

-- Nút Tốc độ nhanh (Preset Speed Buttons)
Tabs.Main:AddButton({
    Title = "⚡ Max Speed (1000)",
    Description = "Tăng nhanh tốc độ lên 1000",
    Callback = function()
        Options.SpeedSlider:SetValue(1000)
    end
})

Tabs.Main:AddButton({
    Title = "🚀 Flash Speed (2500)",
    Description = "Tốc độ siêu nhanh (Có thể gây văng nếu game nhẹ)",
    Callback = function()
        Options.SpeedSlider:SetValue(2500)
    end
})

-- Tự động bật lại khi hồi sinh (Respawn)
game.Players.LocalPlayer.CharacterAdded:Connect(function(char)
    char:WaitForChild("Humanoid")
    task.wait(0.3)
    if SpeedToggle then
        ToggleSpeed(true)
    end
end)

-- Khởi tạo cài đặt
SaveManager:SetLibrary(Fluent)
SaveManager:IgnoreThemeSettings()
SaveManager:BuildConfigSection(Tabs.Settings)
Window:SelectTab(1)

Fluent:Notify({
    Title = "Speed Pro Hub Loaded",
    Content = "Menu đã sẵn sàng! Nhấn Left-Control để ẩn/hiện Menu.",
    Duration = 5
})
