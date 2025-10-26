-- Services
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")

-- Remote usado para atacar (troque se quiser)
local Remote = ReplicatedStorage.NetworkRemotes["063e14b7-326e-48a9-bc5f-48cc2329e10d"]

-- Variável de controle
local auraActive = false

-- Função para atacar jogadores próximos
local function attackNearbyPlayers()
    local localPlayer = Players.LocalPlayer
    local character = localPlayer.Character
    if not character or not character:FindFirstChild("HumanoidRootPart") then return end

    for _, player in pairs(Players:GetPlayers()) do
        if player ~= localPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") and player.Character:FindFirstChild("Humanoid") and player.Character:FindFirstChild("Head") then
            local distance = (character.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).Magnitude
            if distance <= 150 then
                Remote:FireServer(
                    "72*105*116*68*101*65*114*109*97*",
                    player.Character.Humanoid,
                    player.Character.Head,
                    30,
                    player.Character.Head.Position
                )
            end
        end
    end
end

-- Loop de ataque
RunService.RenderStepped:Connect(function()
    if auraActive then
        attackNearbyPlayers()
    end
end)

-- Interface do botão
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
local Button = Instance.new("TextButton", ScreenGui)

Button.Size = UDim2.new(0, 120, 0, 40)
Button.Position = UDim2.new(0.5, -60, 0.9, -20)
Button.Text = "Aura Kill"
Button.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
Button.TextColor3 = Color3.new(1, 1, 1)
Button.Font = Enum.Font.SourceSansBold
Button.TextSize = 20
Button.BorderSizePixel = 0
Button.Active = true
Button.Draggable = true

-- Alternar aura
Button.MouseButton1Click:Connect(function()
    auraActive = not auraActive
    if auraActive then
        Button.Text = "Aura ON"
        Button.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
    else
        Button.Text = "Aura OFF"
        Button.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    end
end)
