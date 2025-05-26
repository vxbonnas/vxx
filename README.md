-- Configuração
local ESP_Ativo = true
local MostrarNomes = true
local MostrarCaixa = true
local VerificarTime = true
local CorAliado = {r = 0, g = 1, b = 0}
local CorInimigo = {r = 1, g = 0, b = 0}

-- Função para desenhar texto na tela
function DesenharTexto(x, y, texto, cor)
    -- Você pode adaptar com sua própria função de renderização
    DrawText(x, y, texto, cor.r, cor.g, cor.b, 1.0)
end

-- Função para desenhar caixa (exemplo simples)
function DesenharCaixa(x, y, largura, altura, cor)
    DrawRect(x, y, largura, altura, cor.r, cor.g, cor.b, 1.0)
end

-- Loop principal
function ESP_Loop()
    if not ESP_Ativo then return end

    for i, jogador in pairs(GetAllPlayers()) do
        if jogador ~= LocalPlayer then
            if VerificarTime and jogador.Team == LocalPlayer.Team then
                cor = CorAliado
            else
                cor = CorInimigo
            end

            local pos3D = jogador.Position
            local pos2D = WorldToScreen(pos3D)
            if pos2D then
                if MostrarNomes then
                    DesenharTexto(pos2D.x, pos2D.y - 15, jogador.Name, cor)
                end
                if MostrarCaixa then
                    DesenharCaixa(pos2D.x, pos2D.y, 50, 100, cor) -- ajuste a largura/altura conforme necessário
                end
            end
        end
    end
end

-- Hook no render (ajuste conforme seu engine)
HookRender(ESP_Loop)
