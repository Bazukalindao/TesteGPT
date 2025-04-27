local UIS = game:GetService("UserInputService")

local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local CloseButton = Instance.new("TextButton")
local UpButton = Instance.new("TextButton")
local DownButton = Instance.new("TextButton")
local PlusButton = Instance.new("TextButton")
local MinusButton = Instance.new("TextButton")
local SpeedLabel = Instance.new("TextLabel")
local FlyButton = Instance.new("TextButton")

local flying = false
local speed = 10
local moveDir = Vector3.new(0, 0, 0)

ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

Frame.Parent = ScreenGui
Frame.Size = UDim2.new(0, 200, 0, 100)
Frame.Position = UDim2.new(0.4, 0, 0.4, 0)
Frame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Frame.BorderSizePixel = 2
Frame.Active = true
Frame.Draggable = true

CloseButton.Parent = Frame
CloseButton.Size = UDim2.new(0, 50, 0, 30)
CloseButton.Position = UDim2.new(0, 0, 0, 0)
CloseButton.Text = "X"
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)

MinusButton.Parent = Frame
MinusButton.Size = UDim2.new(0, 50, 0, 30)
MinusButton.Position = UDim2.new(0, 50, 0, 0)
MinusButton.Text = "-"
MinusButton.BackgroundColor3 = Color3.fromRGB(170, 170, 170)

FlyButton.Parent = Frame
FlyButton.Size = UDim2.new(0, 100, 0, 30)
FlyButton.Position = UDim2.new(0, 100, 0, 0)
FlyButton.Text = "Fastz Hub"
FlyButton.BackgroundColor3 = Color3.fromRGB(200, 200, 200)

UpButton.Parent = Frame
UpButton.Size = UDim2.new(0, 50, 0, 30)
UpButton.Position = UDim2.new(0, 0, 0, 30)
UpButton.Text = "↑"

PlusButton.Parent = Frame
PlusButton.Size = UDim2.new(0, 50, 0, 30)
PlusButton.Position = UDim2.new(0, 50, 0, 30)
PlusButton.Text = "+"

SpeedLabel.Parent = Frame
SpeedLabel.Size = UDim2.new(0, 50, 0, 30)
SpeedLabel.Position = UDim2.new(0, 100, 0, 30)
SpeedLabel.Text = tostring(speed)
SpeedLabel.BackgroundColor3 = Color3.fromRGB(230, 230, 230)

DownButton.Parent = Frame
DownButton.Size = UDim2.new(0, 50, 0, 30)
DownButton.Position = UDim2.new(0, 0, 0, 60)
DownButton.Text = "↓"

local function createBodyGyroAndVelocity()
    local character = game.Players.LocalPlayer.Character
    local hrp = character:FindFirstChild("HumanoidRootPart")
    if hrp then
        if not hrp:FindFirstChild("FlyGyro") then
            local gyro = Instance.new("BodyGyro", hrp)
            gyro.Name = "FlyGyro"
            gyro.MaxTorque = Vector3.new(400000, 400000, 400000)
            gyro.P = 100000
            gyro.CFrame = hrp.CFrame
        end
        if not hrp:FindFirstChild("FlyVelocity") then
            local velocity = Instance.new("BodyVelocity", hrp)
            velocity.Name = "FlyVelocity"
            velocity.MaxForce = Vector3.new(400000, 400000, 400000)
            velocity.Velocity = Vector3.new(0, 0, 0)
        end
    end
end

local function removeBodyGyroAndVelocity()
    local character = game.Players.LocalPlayer.Character
    local hrp = character:FindFirstChild("HumanoidRootPart")
    if hrp then
        if hrp:FindFirstChild("FlyGyro") then
            hrp.FlyGyro:Destroy()
        end
        if hrp:FindFirstChild("FlyVelocity") then
            hrp.FlyVelocity:Destroy()
        end
    end
end

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

FlyButton.MouseButton1Click:Connect(function()
    flying = not flying
    if flying then
        createBodyGyroAndVelocity()
    else
        removeBodyGyroAndVelocity()
    end
end)

PlusButton.MouseButton1Click:Connect(function()
    speed = speed + 1
    SpeedLabel.Text = tostring(speed)
end)

MinusButton.MouseButton1Click:Connect(function()
    if speed > 1 then
        speed = speed - 1
        SpeedLabel.Text = tostring(speed)
    end
end)

UpButton.MouseButton1Click:Connect(function()
    moveDir = Vector3.new(0, 1, 0)
end)

DownButton.MouseButton1Click:Connect(function()
    moveDir = Vector3.new(0, -1, 0)
end)

game:GetService("RunService").Heartbeat:Connect(function()
    if flying then
        local character = game.Players.LocalPlayer.Character
        local hrp = character:FindFirstChild("HumanoidRootPart")
        if hrp and hrp:FindFirstChild("FlyVelocity") then
            hrp.FlyVelocity.Velocity = (game.Workspace.CurrentCamera.CFrame.LookVector * 0) + (moveDir * speed)
            hrp.FlyGyro.CFrame = game.Workspace.CurrentCamera.CFrame
        end
    end
end)

UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        moveDir = Vector3.new(0, 0, 0)
    end
end)
