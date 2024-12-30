-- Script para mostrar os nomes dos jogadores em vermelho sobre as cabeças e desenhar uma linha entre o jogador local e os outros

local jogadorLocal = game.Players.LocalPlayer  -- Jogador que está executando o script (o jogador local)

-- Função para criar a linha e o nome acima da cabeça
local function criarNomeEConectarComLinha(player)
    -- Verifica se o jogador tem uma "Head" (cabeça)
    local personagem = player.Character
    if personagem and personagem:FindFirstChild("Head") then
        -- Cria a parte do texto acima da cabeça
        local head = personagem.Head
        local nomeTag = Instance.new("BillboardGui")
        nomeTag.Parent = head
        nomeTag.Adornee = head
        nomeTag.Size = UDim2.new(0, 200, 0, 50)
        nomeTag.StudsOffset = Vector3.new(0, 2, 0) -- Ajuste de altura
        nomeTag.AlwaysOnTop = true

        -- Cria o texto
        local texto = Instance.new("TextLabel")
        texto.Parent = nomeTag
        texto.Text = player.Name
        texto.TextColor3 = Color3.fromRGB(255, 0, 0) -- Cor vermelha
        texto.TextSize = 20
        texto.BackgroundTransparency = 1 -- Sem fundo

        -- Cria a linha conectando o jogador local com o outro jogador
        if player ~= jogadorLocal then
            local linha = Instance.new("Part")
            linha.Parent = workspace
            linha.Size = Vector3.new(0.2, 0.2, (player.Character.HumanoidRootPart.Position - jogadorLocal.Character.HumanoidRootPart.Position).Magnitude)
            linha.Anchored = true
            linha.CanCollide = false
            linha.BrickColor = BrickColor.new("Bright red")
            linha.Material = Enum.Material.SmoothPlastic

            -- Define a posição inicial e final da linha
            linha.CFrame = CFrame.new(jogadorLocal.Character.HumanoidRootPart.Position, player.Character.HumanoidRootPart.Position) * CFrame.new(0, 0, -(linha.Size.Z / 2))

            -- Conectar a linha para sempre atualizar a posição
            game:GetService("RunService").Heartbeat:Connect(function()
                if player.Character and jogadorLocal.Character then
                    linha.CFrame = CFrame.new(jogadorLocal.Character.HumanoidRootPart.Position, player.Character.HumanoidRootPart.Position) * CFrame.new(0, 0, -(linha.Size.Z / 2))
                    linha.Size = Vector3.new(0.2, 0.2, (player.Character.HumanoidRootPart.Position - jogadorLocal.Character.HumanoidRootPart.Position).Magnitude)
                end
            end)
        end
    end
end

-- Conectar a função para todos os jogadores
game.Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function(character)
        criarNomeEConectarComLinha(player)
    end)
end)

-- Adicionar linha com o jogador local ao entrar no jogo
jogadorLocal.CharacterAdded:Connect(function()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= jogadorLocal then
            criarNomeEConectarComLinha(player)
        end
    end
end)
