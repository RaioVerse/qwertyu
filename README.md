-- Script de Kill Aura para Vox Seas (exemplo educacional)
-- Use apenas para aprendizado! Risco de banimento.

local player = game.Players.LocalPlayer
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local attackRemote = ReplicatedStorage:FindFirstChild("RemoteEventNameAqui") -- Troque pelo nome real do RemoteEvent
local killAuraAtivo = true -- Ativo por padrão
local raio = 30 -- Distância máxima para atacar NPCs

-- Função para verificar se está com melee
local function estaComMelee()
    local char = player.Character
    if not char then return false end
    for _, tool in ipairs(char:GetChildren()) do
        if tool:IsA("Tool") and tool.Name:lower():find("sword") then -- Ajuste o nome se necessário
            return tool
        end
    end
    return nil
end

-- Kill Aura Loop
spawn(function()
    while killAuraAtivo do
        local melee = estaComMelee()
        if melee then
            for _, npc in ipairs(workspace:GetChildren()) do
                if npc:IsA("Model") and npc:FindFirstChild("Humanoid") and npc.Name ~= player.Name then
                    local root = npc:FindFirstChild("HumanoidRootPart") or npc.PrimaryPart
                    if root and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                        local dist = (root.Position - player.Character.HumanoidRootPart.Position).Magnitude
                        if dist <= raio then
                            attackRemote:FireServer(npc) -- Parâmetro pode variar, verifique o correto
                        end
                    end
                end
            end
        end
        wait(0.2) -- Ajuste a frequência do kill aura
    end
end)
