local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ModernHub"
screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
screenGui.ResetOnSpawn = false

-- Khung Main
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 320, 0, 240)
mainFrame.Position = UDim2.new(0.5, -160, 0.4, -120)
mainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 24)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = screenGui

-- Bo góc cho Main Frame
local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 12)
mainCorner.Parent = mainFrame

-- Viền Dạ Quang (Stroke)
local mainStroke = Instance.new("UIStroke")
mainStroke.Color = Color3.fromRGB(90, 80, 255)
mainStroke.Thickness = 1.5
mainStroke.Parent = mainFrame

-- Thanh Tiêu Đề
local titleBar = Instance.new("TextLabel")
titleBar.Size = UDim2.new(1, 0, 0, 45)
titleBar.BackgroundColor3 = Color3.fromRGB(28, 28, 38)
titleBar.Text = "  ⚡ ULTIMATE HUB"
titleBar.TextColor3 = Color3.fromRGB(255, 255, 255)
titleBar.Font = Enum.Font.Garamond
titleBar.TextSize = 20
titleBar.TextXAlignment = Enum.TextXAlignment.Left
titleBar.Parent = mainFrame

local titleCorner = Instance.new("UICorner")
titleCorner.CornerRadius = UDim.new(0, 12)
titleCorner.Parent = titleBar

-- Hàm tạo Nút Bấm Đẹp
local function createButton(text, posy)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.88, 0, 0, 42)
    btn.Position = UDim2.new(0.06, 0, 0, posy)
    btn.BackgroundColor3 = Color3.fromRGB(32, 34, 46)
    btn.Text = text
    btn.TextColor3 = Color3.fromRGB(220, 220, 220)
    btn.Font = Enum.Font.GothamMedium
    btn.TextSize = 14
    btn.Parent = mainFrame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = btn

    local stroke = Instance.new("UIStroke")
    stroke.Color = Color3.fromRGB(50, 52, 70)
    stroke.Thickness = 1
    stroke.Parent = btn

    -- Hiệu ứng Hover chuột
    btn.MouseEnter:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(45, 48, 65)
    end)
    btn.MouseLeave:Connect(function()
        if not btn:GetAttribute("Active") then
            btn.BackgroundColor3 = Color3.fromRGB(32, 34, 46)
        end
    end)

    return btn
end

local godBtn = createButton("🛡️ God Mode: OFF", 65)
local speedBtn = createButton("⚡ Steal Super Speed: OFF", 120)

-- Thêm ghi chú dưới đáy
local note = Instance.new("TextLabel")
note.Size = UDim2.new(1, 0, 0, 20)
note.Position = UDim2.new(0, 0, 1, -25)
note.BackgroundTransparency = 1
note.Text = "Kéo để di chuyển menu"
note.TextColor3 = Color3.fromRGB(120, 120, 140)
note.Font = Enum.Font.Gotham
note.TextSize = 11
note.Parent = mainFrame

-- BIẾN LOGIC
local godActive = false
local speedActive = false
local STEAL_SPEED = 150

-- Logic God Mode
godBtn.MouseButton1Click:Connect(function()
    godActive = not godActive
    godBtn:SetAttribute("Active", godActive)
    if godActive then
        godBtn.Text = "🛡️ God Mode: ON"
        godBtn.BackgroundColor3 = Color3.fromRGB(46, 125, 50)
    else
        godBtn.Text = "🛡️ God Mode: OFF"
        godBtn.BackgroundColor3 = Color3.fromRGB(32, 34, 46)
    end
end)

task.spawn(function()
    while task.wait(0.1) do
        if godActive then
            local char = game.Players.LocalPlayer.Character
            if char and char:FindFirstChild("Humanoid") then
                char.Humanoid.MaxHealth = math.huge
                char.Humanoid.Health = math.huge
            end
        end
    end
end)

-- Logic Super Speed
speedBtn.MouseButton1Click:Connect(function()
    speedActive = not speedActive
    speedBtn:SetAttribute("Active", speedActive)
    if speedActive then
        speedBtn.Text = "⚡ Steal Super Speed: ON"
        speedBtn.BackgroundColor3 = Color3.fromRGB(46, 125, 50)
    else
        speedBtn.Text = "⚡ Steal Super Speed: OFF"
        speedBtn.BackgroundColor3 = Color3.fromRGB(32, 34, 46)
        local char = game.Players.LocalPlayer.Character
        if char and char:FindFirstChild("Humanoid") then
            char.Humanoid.WalkSpeed = 16
        end
    end
end)

-- Duy trì tốc độ chuẩn Steal an Egg
game:GetService("RunService").Stepped:Connect(function()
    if speedActive then
        local char = game.Players.LocalPlayer.Character
        if char and char:FindFirstChild("Humanoid") then
            char.Humanoid.WalkSpeed = STEAL_SPEED
            char.Humanoid.CustomPhysicalProperties = PhysicalProperties.new(0.7, 0.3, 0.5, 1, 1)
        end
    end
end)
