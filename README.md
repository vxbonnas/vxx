local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

function criarESP(jogador)
    local personagem = jogador.Character or jogador.CharacterAdded:Wait()
    if personagem:FindFirstChild("Head") and not personagem.Head:FindFirstChild("ESP") then
        local billboard = Instance.new("BillboardGui")
        billboard.Name = "ESP"
        billboard.Adornee = personagem.Head
        billboard.Size = UDim2.new(0, 100, 0, 30)
        billboard.StudsOffset = Vector3.new(0, 2, 0)
        billboard.AlwaysOnTop = true
        billboard.Parent = personagem.Head

        local texto = Instance.new("TextLabel")
        texto.Size = UDim2.new(1, 0, 1, 0)
        texto.BackgroundTransparency = 1
        texto.Text = jogador.Name
        texto.TextColor3 = Color3.new(1, 0, 0)
        texto.TextStrokeTransparency = 0.5
        texto.TextScaled = true
        texto.Parent = billboard
    end
end

for _, jogador in ipairs(Players:GetPlayers()) do
    if jogador ~= LocalPlayer then
        criarESP(jogador)
    end
end

Players.PlayerAdded:Connect(function(jogador)
    jogador.CharacterAdded:Connect(function()
        criarESP(jogador)
    end)
end)
