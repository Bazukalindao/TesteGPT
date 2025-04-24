local function criarESP(player)
    if player.Character then
        for _, part in pairs(player.Character:GetDescendants()) do
            if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
                local highlight = Instance.new("BoxHandleAdornment")
                highlight.Adornee = part
                highlight.AlwaysOnTop = true
                highlight.ZIndex = 5
                highlight.Size = part.Size
                highlight.Transparency = 0.8
                highlight.Color3 = Color3.new(1, 1, 1)
                highlight.Name = "ESP_Marcado"
                highlight.Parent = part
            end
        end
    end
end

local function removerESP(player)
    if player.Character then
        for _, part in pairs(player.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                local antigoESP = part:FindFirstChild("ESP_Marcado")
                if antigoESP then
                    antigoESP:Destroy()
                end
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

local function desativarESP()
    for _, player in pairs(game:GetService("Players"):GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            removerESP(player)
        end
    end
end

local function criarPainel()
    local gui = Instance.new("ScreenGui", game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui"))
    gui.Name = "PainelESP"

    local frame = Instance.new("Frame", gui)
    frame.Size = UDim2.new(0, 260, 0, 130)
    frame.Position = UDim2.new(0.5, -130, 0.2, 0)
    frame.BackgroundColor3 = Color3.fromRGB(60, 0, 100)
    frame.Active = true
    frame.Draggable = true
    frame.BorderSizePixel = 0

    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 12)

    local titulo = Instance.new("TextLabel", frame)
    titulo.Text = "TESTE ESP"
    titulo.Size = UDim2.new(1, -40, 0, 30)
    titulo.Position = UDim2.new(0, 10, 0, 5)
    titulo.BackgroundTransparency = 1
    titulo.TextColor3 = Color3.new(1, 1, 1)
    titulo.TextScaled = true
    titulo.Font = Enum.Font.GothamBold

    local fechar = Instance.new("TextButton", frame)
    fechar.Text = "X"
    fechar.Size = UDim2.new(0, 30, 0, 30)
    fechar.Position = UDim2.new(1, -35, 0, 5)
    fechar.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
    fechar.TextColor3 = Color3.new(1, 1, 1)
    fechar.Font = Enum.Font.GothamBold
    fechar.TextScaled = true
    Instance.new("UICorner", fechar).CornerRadius = UDim.new(0, 8)

    fechar.MouseButton1Click:Connect(function()
        gui:Destroy()
    end)

    local botaoAtivar = Instance.new("TextButton", frame)
    botaoAtivar.Text = "Ativar ESP"
    botaoAtivar.Size = UDim2.new(0.5, -15, 0, 40)
    botaoAtivar.Position = UDim2.new(0, 10, 0, 45)
    botaoAtivar.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
    botaoAtivar.TextColor3 = Color3.new(1, 1, 1)
    botaoAtivar.Font = Enum.Font.GothamBold
    botaoAtivar.TextScaled = true
    Instance.new("UICorner", botaoAtivar).CornerRadius = UDim.new(0, 8)

    local botaoDesativar = Instance.new("TextButton", frame)
    botaoDesativar.Text = "Desativar ESP"
    botaoDesativar.Size = UDim2.new(0.5, -15, 0, 40)
    botaoDesativar.Position = UDim2.new(0.5, 5, 0, 45)
    botaoDesativar.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
    botaoDesativar.TextColor3 = Color3.new(1, 1, 1)
    botaoDesativar.Font = Enum.Font.GothamBold
    botaoDesativar.TextScaled = true
    Instance.new("UICorner", botaoDesativar).CornerRadius = UDim.new(0, 8)

    botaoAtivar.MouseButton1Click:Connect(ativarESP)
    botaoDesativar.MouseButton1Click:Connect(desativarESP)
end

criarPainel()
