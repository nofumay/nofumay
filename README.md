-- Script ESP aprimorado para Roblox Arsenal



local espEnabled = true -- Variável para ativar/desativar o ESP

local userInputService = game:GetService("UserInputService")

local runService = game:GetService("RunService")



-- Função para criar um quadro ao redor dos jogadores

local function createESP(player, color)

    local highlight = Instance.new("Highlight")

    highlight.Adornee = player.Character

    highlight.FillColor = color

    highlight.FillTransparency = 0.5

    highlight.OutlineTransparency = 0.5

    highlight.Parent = player.Character

end



-- Função para aplicar ESP a todos os jogadores

local function applyESP()

    for _, player in ipairs(game.Players:GetPlayers()) do

        if player.Character then

            if player.TeamColor == game.Players.LocalPlayer.TeamColor then

                createESP(player, Color3.new(0, 0, 1)) -- Azul para aliados

            else

                createESP(player, Color3.new(1, 0, 0)) -- Vermelho para inimigos

            end

        end

    end

end



-- Função para criar setas indicando a posição dos inimigos

local function createArrow(player)

    local arrow = Instance.new("BillboardGui")

    arrow.Adornee = player.Character.HumanoidRootPart

    arrow.Size = UDim2.new(0, 50, 0, 50)

    arrow.AlwaysOnTop = true



    local frame = Instance.new("Frame")

    frame.Size = UDim2.new(1, 0, 1, 0)

    frame.BackgroundTransparency = 0.5

    frame.BackgroundColor3 = Color3.new(1, 0, 0) -- Cor vermelha para setas de inimigos

    frame.Parent = arrow



    arrow.Parent = player.Character

end



-- Função para desenhar FOV e setas

local function drawFOV()

    for _, player in ipairs(game.Players:GetPlayers()) do

        if player.TeamColor ~= game.Players.LocalPlayer.TeamColor and player.Character then

            createArrow(player)

        end

    end

end



-- Função para ativar e desativar o ESP

local function toggleESP(input, gameProcessed)

    if input.KeyCode == Enum.KeyCode.E then

        espEnabled = not espEnabled

        if espEnabled then

            applyESP()

            drawFOV()

        else

            for _, player in ipairs(game.Players:GetPlayers()) do

                if player.Character then

                    local highlight = player.Character:FindFirstChild("Highlight")

                    if highlight then

                        highlight:Destroy()

                    end

                    local arrow = player.Character:FindFirstChildOfClass("BillboardGui")

                    if arrow then

                        arrow:Destroy()

                    end

                end

            end

        end

    end

end



-- Conecta a função ao evento de jogador adicionado

game.Players.PlayerAdded:Connect(function(player)

    player.CharacterAdded:Connect(function()

        if espEnabled then

            applyESP()

            drawFOV()

        end

    end)

end)



-- Aplica ESP e FOV aos jogadores já presentes no jogo

applyESP()

drawFOV()



-- Conecta a função de ativar/desativar ESP ao input do usuário

userInputService.InputBegan:Connect(toggleESP)



-- Atualiza o ESP continuamente

runService.RenderStepped:Connect(function()

    if espEnabled then

        applyESP()

        drawFOV()

    end

end)
