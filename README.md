-- Variáveis principais
local esp_ativo = true
local mostrar_nomes = true
local mostrar_caixa = true
local verificar_time = true

-- Cores
local cor_inimigo = {r=1, g=0, b=0}
local cor_aliado = {r=0, g=1, b=0}

-- Desenhar texto na tela
function desenhar_texto(x, y, texto, cor)
    DrawText(texto, x, y, cor.r, cor.g, cor.b, 1.0)
end

-- Desenhar painel no canto da tela
function desenhar_painel()
    local x = 10
    local y = 30
    desenhar_texto(x, y,      "ESP: " .. tostring(esp_ativo),      {r=1, g=1, b=1})
    desenhar_texto(x, y + 15, "Nomes: " .. tostring(mostrar_nomes), {r=1, g=1, b=1})
    desenhar_texto(x, y + 30, "Caixa: " .. tostring(mostrar_caixa), {r=1, g=1, b=1})
end

-- ESP simples
function esp_loop()
    if not esp_ativo then return end

    for _, jogador in pairs(GetAllPlayers()) do
        if jogador ~= LocalPlayer then
            if not verificar_time or jogador.Team ~= LocalPlayer.Team then
                local pos2D = WorldToScreen(jogador.Position)
                if pos2D then
                    local cor = (jogador.Team == LocalPlayer.Team) and cor_aliado or cor_inimigo

                    if mostrar_nomes then
                        desenhar_texto(pos2D.x, pos2D.y - 20, jogador.Name, cor)
                    end

                    if mostrar_caixa then
                        DrawRect(pos2D.x, pos2D.y, 50, 100, cor.r, cor.g, cor.b, 1.0) -- Caixa simples
                    end
                end
            end
        end
    end
end

-- Loop principal de render
function on_render()
    desenhar_painel()
    esp_loop()
end

-- Adiciona o ESP ao render do jogo
HookRender(on_render)
