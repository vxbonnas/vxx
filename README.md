local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- Criar painel no canto da tela
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ESPPainel"
screenGui.ResetOnSpawn = false
screenGui.Parent = PlayerGui

local label = Instance.new("TextLabel")
label.Size = UDim2.new(0, 200, 0, 25)
label.Position = UDim2.new(0, 10, 0, 10) -- canto superior esquerdo
label.BackgroundTransparency = 0.5
label.BackgroundColor3 = Color3.new(0, 0, 0)
label.Text = "[ESP Ativado]"
label.TextColor3 = Color3.new(0, 1, 0)
label.TextScaled = true
label.Parent = screenGui

-- Função para criar ESP (nome flutuante)
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

-- Aplicar ESP aos jogadores existentes
for _, jogador in ipairs(Players:GetPlayers()) do
    if jogador ~= LocalPlayer then
        criarESP(jogador)
    end
end

-- Aplicar ESP a novos jogadores
Players.PlayerAdded:Connect(function(jogador)
    jogador.CharacterAdded:Connect(function()
        criarESP(jogador)
    end)
end)
