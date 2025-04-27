local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local FlyButton = Instance.new("TextButton")
local SpeedUp = Instance.new("TextButton")
local SpeedDown = Instance.new("TextButton")
local SpeedDisplay = Instance.new("TextLabel")
local CloseButton = Instance.new("TextButton")

local flying = false
local speed = 50

ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
Frame.Parent = ScreenGui
Frame.Size = UDim2.new(0, 200, 0, 120)
Frame.Position = UDim2.new(0.4, 0, 0.3, 0)
Frame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Frame.BorderSizePixel = 2

CloseButton.Parent = Frame
CloseButton.Size = UDim2.new(0, 40, 0, 30)
CloseButton.Position = UDim2.new(0, 0, 0, 0)
CloseButton.Text = "X"
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)

FlyButton.Parent = Frame
FlyButton.Size = UDim2.new(0, 100, 0, 30)
FlyButton.Position = UDim2.new(0, 50, 0, 0)
FlyButton.Text = "Fastz Hub"
FlyButton.BackgroundColor3 = Color3.fromRGB(200, 200, 200)

SpeedUp.Parent = Frame
SpeedUp.Size = UDim2.new(0, 40, 0, 30)
SpeedUp.Position = UDim2.new(0, 0, 0, 40)
SpeedUp.Text = "+"

SpeedDown.Parent = Frame
SpeedDown.Size = UDim2.new(0, 40, 0, 30)
SpeedDown.Position = UDim2.new(0, 50, 0, 40)
SpeedDown.Text = "-"

SpeedDisplay.Parent = Frame
SpeedDisplay.Size = UDim2.new(0, 100, 0, 30)
SpeedDisplay.Position = UDim2.new(0, 100, 0, 40)
SpeedDisplay.Text = tostring(speed)
SpeedDisplay.BackgroundColor3 = Color3.fromRGB(240, 240, 240)

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

SpeedUp.MouseButton1Click:Connect(function()
    speed = speed + 10
    SpeedDisplay.Text = tostring(speed)
end)

SpeedDown.MouseButton1Click:Connect(function()
    if speed > 10 then
        speed = speed - 10
        SpeedDisplay.Text = tostring(speed)
    end
end)

FlyButton.MouseButton1Click:Connect(function()
    local character = game.Players.LocalPlayer.Character
    local humanoidRootPart = character and character:FindFirstChild("HumanoidRootPart")
    
    if humanoidRootPart then
        flying = not flying
        
        if flying then
            humanoidRootPart.Anchored = false
            local bodyForce = Instance.new("BodyForce")
            bodyForce.Name = "FlyForce"
            bodyForce.Force = Vector3.new(0, workspace.Gravity * humanoidRootPart.AssemblyMass + speed, 0)
            bodyForce.Parent = humanoidRootPart
        else
            local existingForce = humanoidRootPart:FindFirstChild("FlyForce")
            if existingForce then
                existingForce:Destroy()
            end
        end
    end
end)

game:GetService("RunService").Heartbeat:Connect(function()
    if flying then
        local character = game.Players.LocalPlayer.Character
        local humanoidRootPart = character and character:FindFirstChild("HumanoidRootPart")
        
        if humanoidRootPart then
            local flyForce = humanoidRootPart:FindFirstChild("FlyForce")
            if flyForce then
                flyForce.Force = Vector3.new(0, workspace.Gravity * humanoidRootPart.AssemblyMass + speed, 0)
            end
        end
    end
end)
