-- Configurações iniciais
local ESP_Ativo = true
local MostrarNomes = true
local MostrarCaixa = true
local VerificarTime = true

-- Cores
local CorAliado = {r = 0, g = 1, b = 0}
local CorInimigo = {r = 1, g = 0, b = 0}

-- Teclas de atalho (substitua se necessário)
local TeclaESP = "F1"
local TeclaNomes = "F2"
local TeclaCaixa = "F3"

-- Função para desenhar texto (adapte ao seu renderizador)
function DesenharTexto(x, y, texto, r, g, b)
    -- Função genérica (substitua por sua API de renderização)
    DrawText(tostring(texto), x, y, r, g, b, 1.0)
end

-- Painel de status no canto da tela
function MostrarPainel()
    local baseX = 20
    local baseY = 40
    local distY = 15

    DesenharTexto(baseX, baseY,     "ESP: " .. tostring(ESP_Ativo),      1, 1, 1)
    DesenharTexto(baseX, baseY+distY, "Nomes: " .. tostring(MostrarNomes), 1, 1, 1)
    DesenharTexto(baseX, baseY+distY*2, "Caixas: " .. tostring(MostrarCaixa), 1, 1, 1)
end

-- Render ESP
function ESP_Render()
    if not ESP_Ativo then return end

    for i, jogador in pairs(GetAllPlayers()) do
        if jogador ~= LocalPlayer then
            -- Team Check
            local cor = CorInimigo
            if VerificarTime and jogador.Team == LocalPlayer.Team then
                cor = CorAliado
            end

            -- Posição do jogador
            local pos3D = jogador.Position
            local pos2D = WorldToScreen(pos3D)
            if pos2D then
                if MostrarNomes then
                    DesenharTexto(pos2D.x, pos2D.y - 20, jogador.Name, cor.r, cor.g, cor.b)
                end
                if MostrarCaixa then
                    DrawRect(pos2D.x, pos2D.y, 50, 100, cor.r, cor.g, cor.b, 1.0) -- Ajuste tamanho
                end
            end
        end
    end
end

-- Monitoramento de teclas
function ChecarTeclas()
    if IsKeyPressed(TeclaESP) then
        ESP_Ativo = not ESP_Ativo
    end
    if IsKeyPressed(TeclaNomes) then
        MostrarNomes = not MostrarNomes
    end
    if IsKeyPressed(TeclaCaixa) then
        MostrarCaixa = not MostrarCaixa
    end
end

-- Loop principal
function OnRender()
    ChecarTeclas()
    MostrarPainel()
    ESP_Render()
end

-- Hook no render (ou substitua pelo método do seu jogo)
HookRender(OnRender)

