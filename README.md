--[[
        WARNING: Heads up! This script has not been verified by BloxLord . Use at your own risk!
]] 

local CoreGui = game:GetService("CoreGui")
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")

local uiParent = pcall(function() return CoreGui.Name end) and CoreGui or Players.LocalPlayer:WaitForChild("PlayerGui")

if uiParent:FindFirstChild("GlitchSystemsUI") then
    uiParent.GlitchSystemsUI:Destroy()
end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "GlitchSystemsUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = uiParent

-- Khung Main (Tăng chiều cao lên 360 để vừa thêm 2 nút mới)
local mainFrame = Instance.new("Frame")
mainFrame.Name = "Forsaken"
mainFrame.Size = UDim2.new(0, 400, 0, 360)
mainFrame.Position = UDim2.new(0.5, -200, 0.5, -180)
mainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
mainFrame.BackgroundTransparency = 0.2 
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = screenGui

local uiCorner = Instance.new("UICorner")
uiCorner.CornerRadius = UDim.new(0, 8)
uiCorner.Parent = mainFrame

local uiStroke = Instance.new("UIStroke")
uiStroke.Thickness = 2
uiStroke.Parent = mainFrame

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0, 50)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "Made by Glitch & NoHyped"
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextSize = 22
titleLabel.Parent = mainFrame

-- 1. NÚT AUTO BLOCK V2
local autoBlockBtn = Instance.new("TextButton")
autoBlockBtn.Size = UDim2.new(0, 280, 0, 45)
autoBlockBtn.Position = UDim2.new(0.5, -140, 0, 65)
autoBlockBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
autoBlockBtn.BackgroundTransparency = 0.3
autoBlockBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
autoBlockBtn.Font = Enum.Font.GothamSemibold
autoBlockBtn.TextSize = 16
autoBlockBtn.Text = "Auto Block V2"
autoBlockBtn.AutoButtonColor = true
autoBlockBtn.Parent = mainFrame

local btnCorner1 = Instance.new("UICorner")
btnCorner1.CornerRadius = UDim.new(0, 6)
btnCorner1.Parent = autoBlockBtn

local btnStroke1 = Instance.new("UIStroke")
btnStroke1.Thickness = 1.5
btnStroke1.Parent = autoBlockBtn

-- 2. NÚT AUTO HỒI MÁU (GOD MODE)
local healBtn = Instance.new("TextButton")
healBtn.Size = UDim2.new(0, 280, 0, 45)
healBtn.Position = UDim2.new(0.5, -140, 0, 130)
healBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
healBtn.BackgroundTransparency = 0.3
healBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
healBtn.Font = Enum.Font.GothamSemibold
healBtn.TextSize = 16
healBtn.Text = "Auto Hồi Máu: OFF"
healBtn.AutoButtonColor = true
healBtn.Parent = mainFrame

local btnCorner2 = Instance.new("UICorner")
btnCorner2.CornerRadius = UDim.new(0, 6)
btnCorner2.Parent = healBtn

local btnStroke2 = Instance.new("UIStroke")
btnStroke2.Thickness = 1.5
btnStroke2.Parent = healBtn

-- 3. NÚT SUPER SPEED
local speedBtn = Instance.new("TextButton")
speedBtn.Size = UDim2.new(0, 280, 0, 45)
speedBtn.Position = UDim2.new(0.5, -140, 0, 195)
speedBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
speedBtn.BackgroundTransparency = 0.3
speedBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
speedBtn.Font = Enum.Font.GothamSemibold
speedBtn.TextSize = 16
speedBtn.Text = "Super Speed: OFF"
speedBtn.AutoButtonColor = true
speedBtn.Parent = mainFrame

local btnCorner3 = Instance.new("UICorner")
btnCorner3.CornerRadius = UDim.new(0, 6)
btnCorner3.Parent = speedBtn

local btnStroke3 = Instance.new("UIStroke")
btnStroke3.Thickness = 1.5
btnStroke3.Parent = speedBtn

-- HIỆU ỨNG RAINBOW ĐỔI MÀU CHO TẤT CẢ VIỀN
local hue = 0
RunService.RenderStepped:Connect(function(deltaTime)
    hue = hue + (deltaTime * 0.15)
    if hue > 1 then 
        hue = 0 
    end
    
    local rainbowColor = Color3.fromHSV(hue, 1, 1)
    
    uiStroke.Color = rainbowColor
    btnStroke1.Color = rainbowColor
    btnStroke2.Color = rainbowColor
    btnStroke3.Color = rainbowColor
    titleLabel.TextColor3 = rainbowColor
end)

-- LOGIC AUTO BLOCK V2 GỐC
autoBlockBtn.MouseButton1Click:Connect(function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/FortheLolzahaha-alt/ForsakenScriptV2/refs/heads/main/By%20Glitch%20%26%20NoHyped"))()
end)

-- LOGIC AUTO HỒI MÁU
local autoHealActive = false
healBtn.MouseButton1Click:Connect(function()
    autoHealActive = not autoHealActive
    if autoHealActive then
        healBtn.Text = "Auto Hồi Máu: ON"
        healBtn.TextColor3 = Color3.fromRGB(100, 255, 100)
    else
        healBtn.Text = "Auto Hồi Máu: OFF"
        healBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    end
end)

-- Vòng lặp liên tục duy trì máu
task.spawn(function()
    while task.wait(0.1) do
        if autoHealActive then
            local char = Players.LocalPlayer.Character
            if char and char:FindFirstChild("Humanoid") then
                local humanoid = char.Humanoid
                humanoid.MaxHealth = math.huge
                humanoid.Health = math.huge
            end
        end
    end
end)

-- LOGIC SUPER SPEED
local superSpeedActive = false
local SPEED_VALUE = 150 -- Tốc độ di chuyển (có thể chỉnh lại theo ý muốn)

speedBtn.MouseButton1Click:Connect(function()
    superSpeedActive = not superSpeedActive
    if superSpeedActive then
        speedBtn.Text = "Super Speed: ON"
        speedBtn.TextColor3 = Color3.fromRGB(100, 255, 100)
    else
        speedBtn.Text = "Super Speed: OFF"
        speedBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
        
        local char = Players.LocalPlayer.Character
        if char and char:FindFirstChild("Humanoid") then
            char.Humanoid.WalkSpeed = 16
        end
    end
end)

-- Vòng lặp duy trì tốc độ (phong cách Steal an Egg)
RunService.Stepped:Connect(function()
    if superSpeedActive then
        local char = Players.LocalPlayer.Character
        if char and char:FindFirstChild("Humanoid") then
            char.Humanoid.WalkSpeed = SPEED_VALUE
            char.Humanoid.CustomPhysicalProperties = PhysicalProperties.new(0.7, 0.3, 0.5, 1, 1)
        end
    end
end)
