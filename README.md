local ativar_esp = true
local verificar_time = true

-- Cor para aliados e inimigos
local cor_aliado = {r=0, g=1, b=0}
local cor_inimigo = {r=1, g=0, b=0}

function desenhar_texto(x, y, texto, cor)
    DrawText(texto, x, y, cor.r, cor.g, cor.b, 1.0)
end

function esp_simples()
    if not ativar_esp then return end

    for _, jogador in pairs(GetAllPlayers()) do
        if jogador ~= LocalPlayer then
            local mesmo_time = (jogador.Team == LocalPlayer.Team)
            if not verificar_time or not mesmo_time then
                local pos = WorldToScreen(jogador.Position)
                if pos then
                    local cor = mesmo_time and cor_aliado or cor_inimigo
                    desenhar_texto(pos.x, pos.y, jogador.Name, cor)
                end
            end
        end
    end
end

HookRender(esp_simples)
