local function criarPainel()
    local ScreenGui = Instance.new("ScreenGui", game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui"))
    ScreenGui.Name = "PainelESP"

    local Frame = Instance.new("Frame", ScreenGui)
    Frame.Size = UDim2.new(0, 250, 0, 100)
    Frame.Position = UDim2.new(0.5, -125, 0.2, 0)
    Frame.BackgroundColor3 = Color3.fromRGB(60, 0, 100)
    Frame.Active = true
    Frame.Draggable = true
    Frame.BorderSizePixel = 0

    local UICorner = Instance.new("UICorner", Frame)
    UICorner.CornerRadius = UDim.new(0, 12)

    local Titulo = Instance.new("TextLabel", Frame)
    Titulo.Text = "TESTE ESP"
    Titulo.Size = UDim2.new(1, -40, 0, 30)
    Titulo.Position = UDim2.new(0, 10, 0, 5)
    Titulo.BackgroundTransparency = 1
    Titulo.TextColor3 = Color3.new(1, 1, 1)
    Titulo.TextScaled = true
    Titulo.Font = Enum.Font.GothamBold

    local BotaoFechar = Instance.new("TextButton", Frame)
    BotaoFechar.Text = "X"
    BotaoFechar.Size = UDim2.new(0, 30, 0, 30)
    BotaoFechar.Position = UDim2.new(1, -35, 0, 5)
    BotaoFechar.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
    BotaoFechar.TextColor3 = Color3.new(1, 1, 1)
    BotaoFechar.Font = Enum.Font.GothamBold
    BotaoFechar.TextScaled = true

    local UICornerBotao = Instance.new("UICorner", BotaoFechar)
    UICornerBotao.CornerRadius = UDim.new(0, 8)

    BotaoFechar.MouseButton1Click:Connect(function()
        ScreenGui:Destroy()
    end)
end

criarPainel()
