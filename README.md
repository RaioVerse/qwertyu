-- Script para atacar NPCs à distância em Vox Seas (exemplo educacional)

local player = game.Players.LocalPlayer
local mouse = player:GetMouse()
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local attackRemote = ReplicatedStorage:FindFirstChild("RemoteEventNameAqui") -- Troque pelo nome real do RemoteEvent

-- Função para verificar se o player está com melee na mão
local function estaComMelee()
    local char = player.Character
    if not char then return false end
    for _, obj in ipairs(char:GetChildren()) do
        if obj:IsA("Tool") and obj.Name:lower():find("sword") then -- Troque por nome da melee se souber
            return obj
        end
    end
    return nil
end

-- Função para atacar todos NPCs próximos
local function atacarNPCs()
    local melee = estaComMelee()
    if not melee then
        warn("Equipe uma melee primeiro!")
        return
    end
    for _, npc in ipairs(workspace:GetChildren()) do
        if npc:IsA("Model") and npc:FindFirstChild("Humanoid") and npc.Name ~= player.Name then
            -- Envie ataque para o servidor como se estivesse perto do NPC
            attackRemote:FireServer(npc) -- Parâmetros podem variar, adapte conforme o jogo
        end
    end
end

-- Ativa com a tecla Z
mouse.KeyDown:Connect(function(key)
    if key:lower() == "z" then
        atacarNPCs()
    end
end)
