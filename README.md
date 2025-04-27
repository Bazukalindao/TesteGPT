-- Universal Mobile Fly Script com Botões Fly/Stop
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local StarterGui = game:GetService("StarterGui")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

local flying = false
local speed = 40
local bodyGyro, bodyVelocity

-- Criar botões na tela
local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
screenGui.Name = "FlyGUI"

local flyButton = Instance.new("TextButton", screenGui)
flyButton.Size = UDim2.new(0, 120, 0, 50)
flyButton.Position = UDim2.new(0.05, 0, 0.8, 0)
flyButton.Text = "Fly"
flyButton.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
flyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
flyButton.TextScaled = true
flyButton.Font = Enum.Font.SourceSansBold
flyButton.Visible = true

local stopButton = Instance.new("TextButton", screenGui)
stopButton.Size = UDim2.new(0, 120, 0, 50)
stopButton.Position = UDim2.new(0.05, 0, 0.88, 0)
stopButton.Text = "Stop Fly"
stopButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
stopButton.TextColor3 = Color3.fromRGB(255, 255, 255)
stopButton.TextScaled = true
stopButton.Font = Enum.Font.SourceSansBold
stopButton.Visible = true

-- Funções Fly
local function startFly()
    if flying then return end
    flying = true
    
    bodyGyro = Instance.new("BodyGyro")
    bodyGyro.P = 9e4
    bodyGyro.Parent = humanoidRootPart

    bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.Velocity = Vector3.zero
    bodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
    bodyVelocity.Parent = humanoidRootPart
end

local function stopFly()
    flying = false
    if bodyGyro then bodyGyro:Destroy() end
    if bodyVelocity then bodyVelocity:Destroy() end
end

-- Conectar botões
flyButton.MouseButton1Click:Connect(function()
    startFly()
end)

stopButton.MouseButton1Click:Connect(function()
    stopFly()
end)

-- Atualizar Fly
RunService.RenderStepped:Connect(function()
    if flying and bodyVelocity and bodyGyro then
        local cam = workspace.CurrentCamera
        local moveDirection = cam.CFrame.LookVector

        bodyVelocity.Velocity = moveDirection * speed
        bodyGyro.CFrame = cam.CFrame
    end
end)
