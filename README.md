local ScreenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
ScreenGui.Name = "PainelESP"

local Frame = Instance.new("Frame", ScreenGui)
Frame.Name = "ESP TESTE"
Frame.Size = UDim2.new(0, 300, 0, 130)
Frame.Position = UDim2.new(0.5, -150, 0.2, 0)
Frame.BackgroundColor3 = Color3.fromRGB(128, 0, 255)
Frame.Active = true
Frame.Draggable = true
Frame.BorderSizePixel = 0
Frame.BackgroundTransparency = 0.05

local CloseBtn = Instance.new("TextButton", Frame)
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -35, 0, 5)
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.new(1, 1, 1)
CloseBtn.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextScaled = true
CloseBtn.BorderSizePixel = 0

CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

local AtivarBtn = Instance.new("TextButton", Frame)
AtivarBtn.Size = UDim2.new(0, 260, 0, 60)
AtivarBtn.Position = UDim2.new(0.5, -130, 0.5, -10)
AtivarBtn.Text = "Ativar ESP"
AtivarBtn.TextColor3 = Color3.new(1, 1, 1)
AtivarBtn.BackgroundColor3 = Color3.fromRGB(85, 0, 170)
AtivarBtn.Font = Enum.Font.GothamBold
AtivarBtn.TextScaled = true
AtivarBtn.BorderSizePixel = 0

local function criarESP(player)
    if player.Character then
        if not player.Character:FindFirstChild("ESP_Highlight") then
            local highlight = Instance.new("Highlight")
            highlight.Name = "ESP_Highlight"
            highlight.FillColor = Color3.new(1, 1, 1)
            highlight.FillTransparency = 0.7
            highlight.OutlineTransparency = 1
            highlight.Adornee = player.Character
            highlight.Parent = player.Character
        end

        if not player.Character:FindFirstChild("ESP_Nome") then
            local head = player.Character:FindFirstChild("Head")
            if head then
                local billboard = Instance.new("BillboardGui")
                billboard.Name = "ESP_Nome"
                billboard.Adornee = head
                billboard.Size = UDim2.new(0, 200, 0, 50)
                billboard.StudsOffset = Vector3.new(0, 2, 0)
                billboard.AlwaysOnTop = true
                billboard.Parent = player.Character

                local texto = Instance.new("TextLabel", billboard)
                texto.Size = UDim2.new(1, 0, 1, 0)
                texto.BackgroundTransparency = 1
                texto.Text = player.Name
                texto.TextColor3 = Color3.new(1, 1, 1)
                texto.TextStrokeTransparency = 0.5
                texto.TextScaled = true
                texto.Font = Enum.Font.GothamBold
            end
        end
    end
end

local function ativarESP()
    for _, player in pairs(game:GetService("Players"):GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            criarESP(player)
            player.CharacterAdded:Connect(function()
                wait(1)
                criarESP(player)
            end)
        end
    end
end

AtivarBtn.MouseButton1Click:Connect(ativarESP)
