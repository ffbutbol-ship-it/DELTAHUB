-- Delta Executor Mobile Script - CORRIGIDO V4
-- Floating UI with AimBot, ESP and Hacker features

local player = game.Players.LocalPlayer
local mouse = player:GetMouse()
local runService = game:GetService("RunService")
local userInputService = game:GetService("UserInputService")
local camera = workspace.CurrentCamera
local players = game:GetService("Players")
local httpService = game:GetService("HttpService")
local starterGui = game:GetService("StarterGui")

-- Aguardar o personagem carregar
repeat wait() until player.Character
repeat wait() until player.Character:FindFirstChild("HumanoidRootPart")

-- Sistema de Key
local VALID_KEY = "free_lkzinnfovbrab0x111"
local keyVerified = false
local scriptLoaded = false

-- Create ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DeltaHub"
screenGui.Parent = game.CoreGui
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- ============ TELA DE KEY ============
local keyFrame = Instance.new("Frame")
keyFrame.Name = "KeyFrame"
keyFrame.Size = UDim2.new(0, 400, 0, 550)
keyFrame.Position = UDim2.new(0.5, -200, 0.5, -275)
keyFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
keyFrame.BorderSizePixel = 0
keyFrame.Visible = true
keyFrame.Parent = screenGui

-- Imagem de fundo da Key COBRINDO TODA A ÁREA
local keyImage = Instance.new("ImageLabel")
keyImage.Size = UDim2.new(1, 0, 0, 250)
keyImage.Position = UDim2.new(0, 0, 0, 0)
keyImage.BackgroundTransparency = 1
keyImage.Image = "rbxassetid://118215876813188"
keyImage.ScaleType = Enum.ScaleType.Crop
keyImage.Parent = keyFrame

-- Título
local keyTitle = Instance.new("TextLabel")
keyTitle.Size = UDim2.new(1, 0, 0, 40)
keyTitle.Position = UDim2.new(0, 0, 0, 260)
keyTitle.BackgroundTransparency = 1
keyTitle.Text = "DELTA HUB - VERIFICAÇÃO"
keyTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
keyTitle.Font = Enum.Font.SourceSansBold
keyTitle.TextSize = 22
keyTitle.Parent = keyFrame

-- Campo de Key
local keyInputLabel = Instance.new("TextLabel")
keyInputLabel.Size = UDim2.new(0.8, 0, 0, 25)
keyInputLabel.Position = UDim2.new(0.1, 0, 0, 310)
keyInputLabel.BackgroundTransparency = 1
keyInputLabel.Text = "DIGITE SUA KEY:"
keyInputLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
keyInputLabel.Font = Enum.Font.SourceSans
keyInputLabel.TextSize = 14
keyInputLabel.TextXAlignment = Enum.TextXAlignment.Left
keyInputLabel.Parent = keyFrame

local keyInput = Instance.new("TextBox")
keyInput.Size = UDim2.new(0.8, 0, 0, 35)
keyInput.Position = UDim2.new(0.1, 0, 0, 340)
keyInput.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
keyInput.Text = ""
keyInput.TextColor3 = Color3.fromRGB(255, 255, 255)
keyInput.Font = Enum.Font.SourceSans
keyInput.TextSize = 16
keyInput.PlaceholderText = "Cole sua key aqui..."
keyInput.PlaceholderColor3 = Color3.fromRGB(150, 150, 150)
keyInput.Parent = keyFrame

-- Botão Verificar Key
local verifyButton = Instance.new("TextButton")
verifyButton.Size = UDim2.new(0.8, 0, 0, 40)
verifyButton.Position = UDim2.new(0.1, 0, 0, 385)
verifyButton.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
verifyButton.Text = "VERIFICAR KEY"
verifyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
verifyButton.Font = Enum.Font.SourceSansBold
verifyButton.TextSize = 16
verifyButton.Parent = keyFrame

-- Status da Key
local keyStatus = Instance.new("TextLabel")
keyStatus.Size = UDim2.new(0.8, 0, 0, 25)
keyStatus.Position = UDim2.new(0.1, 0, 0, 435)
keyStatus.BackgroundTransparency = 1
keyStatus.Text = ""
keyStatus.TextColor3 = Color3.fromRGB(255, 255, 255)
keyStatus.Font = Enum.Font.SourceSans
keyStatus.TextSize = 14
keyStatus.Parent = keyFrame

-- Separador
local separator = Instance.new("Frame")
separator.Size = UDim2.new(0.8, 0, 0, 1)
separator.Position = UDim2.new(0.1, 0, 0, 470)
separator.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
separator.Parent = keyFrame

-- Opção de pegar Key via Google/Gmail
local getKeyLabel = Instance.new("TextLabel")
getKeyLabel.Size = UDim2.new(0.8, 0, 0, 25)
getKeyLabel.Position = UDim2.new(0.1, 0, 0, 480)
getKeyLabel.BackgroundTransparency = 1
getKeyLabel.Text = "NÃO TEM KEY? PEGUE VIA GMAIL"
getKeyLabel.TextColor3 = Color3.fromRGB(255, 200, 0)
getKeyLabel.Font = Enum.Font.SourceSansBold
getKeyLabel.TextSize = 16
getKeyLabel.Parent = keyFrame

-- Campo de Email
local emailInput = Instance.new("TextBox")
emailInput.Size = UDim2.new(0.8, 0, 0, 30)
emailInput.Position = UDim2.new(0.1, 0, 0, 510)
emailInput.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
emailInput.Text = ""
emailInput.TextColor3 = Color3.fromRGB(255, 255, 255)
emailInput.Font = Enum.Font.SourceSans
emailInput.TextSize = 14
emailInput.PlaceholderText = "Digite seu Gmail..."
emailInput.PlaceholderColor3 = Color3.fromRGB(150, 150, 150)
emailInput.Parent = keyFrame

-- Botão Pegar Key
local getKeyButton = Instance.new("TextButton")
getKeyButton.Size = UDim2.new(0.8, 0, 0, 35)
getKeyButton.Position = UDim2.new(0.1, 0, 0, 550)
getKeyButton.BackgroundColor3 = Color3.fromRGB(219, 68, 55)
getKeyButton.Text = "ENVIAR KEY PARA GMAIL"
getKeyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
getKeyButton.Font = Enum.Font.SourceSansBold
getKeyButton.TextSize = 14
getKeyButton.Parent = keyFrame

-- Função para verificar Key
local function checkKey(inputKey)
    if inputKey == VALID_KEY then
        keyVerified = true
        keyStatus.Text = "✅ Key válida! Carregando script..."
        keyStatus.TextColor3 = Color3.fromRGB(0, 255, 0)
        wait(1)
        keyFrame.Visible = false
        loadScript()
    else
        keyStatus.Text = "❌ Key inválida! Tente novamente"
        keyStatus.TextColor3 = Color3.fromRGB(255, 0, 0)
        keyInput.Text = ""
    end
end

-- Botão Verificar
verifyButton.MouseButton1Click:Connect(function()
    local inputKey = keyInput.Text
    if inputKey ~= "" then
        checkKey(inputKey)
    else
        keyStatus.Text = "⚠️ Digite uma key!"
        keyStatus.TextColor3 = Color3.fromRGB(255, 255, 0)
    end
end)

-- Sistema de envio para Gmail
getKeyButton.MouseButton1Click:Connect(function()
    local email = emailInput.Text
    
    if email == "" or not email:find("@gmail.com") then
        keyStatus.Text = "⚠️ Digite um Gmail válido!"
        keyStatus.TextColor3 = Color3.fromRGB(255, 255, 0)
        return
    end
    
    keyStatus.Text = "🔄 Enviando para " .. email .. "..."
    keyStatus.TextColor3 = Color3.fromRGB(255, 255, 0)
    
    -- Simular envio para Gmail via webhook
    pcall(function()
        local webhookURL = "https://discord.com/api/webhooks/SEU_WEBHOOK_AQUI"
        local embed = {
            ["title"] = "📧 Nova Key Solicitada",
            ["description"] = "**Email:** " .. email .. "\n**Key:** " .. VALID_KEY .. "\n**Jogador:** " .. player.Name,
            ["color"] = 65280,
            ["footer"] = {
                ["text"] = "Delta Hub - Sistema de Key"
            }
        }
        local data = {
            ["content"] = "Nova solicitação de Key!",
            ["embeds"] = {embed}
        }
        httpService:PostAsync(webhookURL, httpService:JSONEncode(data))
    end)
    
    wait(2)
    
    -- Mostrar notificação simulando Gmail
    starterGui:SetCore("SendNotification", {
        Title = "📬 Gmail - Nova Mensagem",
        Text = "Sua Key foi enviada para " .. email .. "\nKey: " .. VALID_KEY,
        Duration = 10,
        Button1 = "OK",
        Button2 = "Copiar Key"
    })
    
    wait(0.5)
    
    keyStatus.Text = "✅ Key enviada! Verifique seu Gmail"
    keyStatus.TextColor3 = Color3.fromRGB(0, 255, 0)
    
    -- Preencher automaticamente
    keyInput.Text = VALID_KEY
    
    wait(1)
    keyStatus.Text = "📋 Key: " .. VALID_KEY .. " (já preenchida)"
end)

-- Permitir Enter para verificar
keyInput.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        local inputKey = keyInput.Text
        if inputKey ~= "" then
            checkKey(inputKey)
        end
    end
end)

-- ============ SCRIPT PRINCIPAL ============
function loadScript()
    if scriptLoaded then return end
    scriptLoaded = true
    
    -- Notificação de boas-vindas
    starterGui:SetCore("SendNotification", {
        Title = "Delta Hub",
        Text = "Script carregado com sucesso!",
        Duration = 5,
    })
    
    -- Create Floating Button
    local floatingButton = Instance.new("ImageButton")
    floatingButton.Size = UDim2.new(0, 60, 0, 60)
    floatingButton.Position = UDim2.new(0.8, 0, 0.5, 0)
    floatingButton.BackgroundTransparency = 1
    floatingButton.Image = "rbxassetid://104332463494368"
    floatingButton.Parent = screenGui
    floatingButton.Draggable = true
    floatingButton.Active = true

    -- Create Main Panel
    local mainFrame = Instance.new("Frame")
    mainFrame.Name = "MainFrame"
    mainFrame.Size = UDim2.new(0, 350, 0, 400)
    mainFrame.Position = UDim2.new(0.5, -175, 0.5, -200)
    mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    mainFrame.BorderSizePixel = 0
    mainFrame.Visible = false
    mainFrame.Parent = screenGui

    -- Make panel draggable
    local dragging
    local dragInput
    local dragStart
    local startPos

    local function updateDrag(input)
        local delta = input.Position - dragStart
        mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end

    mainFrame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = mainFrame.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    mainFrame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)

    userInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            updateDrag(input)
        end
    end)

    -- Title Bar
    local titleBar = Instance.new("Frame")
    titleBar.Size = UDim2.new(1, 0, 0, 40)
    titleBar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    titleBar.BorderSizePixel = 0
    titleBar.Parent = mainFrame

    local titleText = Instance.new("TextLabel")
    titleText.Size = UDim2.new(0.8, 0, 1, 0)
    titleText.Position = UDim2.new(0.05, 0, 0, 0)
    titleText.BackgroundTransparency = 1
    titleText.Text = "Delta Hub"
    titleText.TextColor3 = Color3.fromRGB(255, 255, 255)
    titleText.Font = Enum.Font.SourceSansBold
    titleText.TextSize = 20
    titleText.TextXAlignment = Enum.TextXAlignment.Left
    titleText.Parent = titleBar

    local closeButton = Instance.new("TextButton")
    closeButton.Size = UDim2.new(0, 40, 0, 40)
    closeButton.Position = UDim2.new(0.9, 0, 0, 0)
    closeButton.BackgroundTransparency = 1
    closeButton.Text = "X"
    closeButton.TextColor3 = Color3.fromRGB(255, 0, 0)
    closeButton.Font = Enum.Font.SourceSansBold
    closeButton.TextSize = 24
    closeButton.Parent = titleBar

    -- Tab Buttons
    local tabHolder = Instance.new("Frame")
    tabHolder.Size = UDim2.new(1, 0, 0, 35)
    tabHolder.Position = UDim2.new(0, 0, 0, 40)
    tabHolder.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    tabHolder.BorderSizePixel = 0
    tabHolder.Parent = mainFrame

    local aimbotTab = Instance.new("TextButton")
    aimbotTab.Size = UDim2.new(0.33, -2, 1, 0)
    aimbotTab.Position = UDim2.new(0, 0, 0, 0)
    aimbotTab.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    aimbotTab.Text = "AimBot"
    aimbotTab.TextColor3 = Color3.fromRGB(255, 255, 255)
    aimbotTab.Font = Enum.Font.SourceSans
    aimbotTab.TextSize = 16
    aimbotTab.Parent = tabHolder

    local espTab = Instance.new("TextButton")
    espTab.Size = UDim2.new(0.33, -2, 1, 0)
    espTab.Position = UDim2.new(0.335, 0, 0, 0)
    espTab.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    espTab.Text = "ESP"
    espTab.TextColor3 = Color3.fromRGB(255, 255, 255)
    espTab.Font = Enum.Font.SourceSans
    espTab.TextSize = 16
    espTab.Parent = tabHolder

    local hackerTab = Instance.new("TextButton")
    hackerTab.Size = UDim2.new(0.33, -2, 1, 0)
    hackerTab.Position = UDim2.new(0.67, 0, 0, 0)
    hackerTab.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    hackerTab.Text = "Hacker"
    hackerTab.TextColor3 = Color3.fromRGB(255, 255, 255)
    hackerTab.Font = Enum.Font.SourceSans
    hackerTab.TextSize = 16
    hackerTab.Parent = tabHolder

    -- Content Frames
    local aimbotContent = Instance.new("ScrollingFrame")
    aimbotContent.Size = UDim2.new(1, 0, 1, -75)
    aimbotContent.Position = UDim2.new(0, 0, 0, 75)
    aimbotContent.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    aimbotContent.BorderSizePixel = 0
    aimbotContent.Visible = true
    aimbotContent.ScrollBarThickness = 5
    aimbotContent.Parent = mainFrame

    local espContent = Instance.new("ScrollingFrame")
    espContent.Size = UDim2.new(1, 0, 1, -75)
    espContent.Position = UDim2.new(0, 0, 0, 75)
    espContent.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    espContent.BorderSizePixel = 0
    espContent.Visible = false
    espContent.ScrollBarThickness = 5
    espContent.Parent = mainFrame

    local hackerContent = Instance.new("ScrollingFrame")
    hackerContent.Size = UDim2.new(1, 0, 1, -75)
    hackerContent.Position = UDim2.new(0, 0, 0, 75)
    hackerContent.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    hackerContent.BorderSizePixel = 0
    hackerContent.Visible = false
    hackerContent.ScrollBarThickness = 5
    hackerContent.Parent = mainFrame

    -- Variables
    local aimbotEnabled = false
    local fovCircle = nil
    local fovSize = 200
    local espEnabled = {line = false, box = false, health = false, skeleton = false}
    local noclipEnabled = false
    local infiniteJumpEnabled = false
    local speedValue = 16
    local espConnections = {}
    local aimbotConnection = nil

    -- Função para alternar abas
    local function switchTab(tab)
        aimbotContent.Visible = (tab == "aimbot")
        espContent.Visible = (tab == "esp")
        hackerContent.Visible = (tab == "hacker")
        
        aimbotTab.BackgroundColor3 = (tab == "aimbot") and Color3.fromRGB(35, 35, 35) or Color3.fromRGB(30, 30, 30)
        espTab.BackgroundColor3 = (tab == "esp") and Color3.fromRGB(35, 35, 35) or Color3.fromRGB(30, 30, 30)
        hackerTab.BackgroundColor3 = (tab == "hacker") and Color3.fromRGB(35, 35, 35) or Color3.fromRGB(30, 30, 30)
    end

    aimbotTab.MouseButton1Click:Connect(function() switchTab("aimbot") end)
    espTab.MouseButton1Click:Connect(function() switchTab("esp") end)
    hackerTab.MouseButton1Click:Connect(function() switchTab("hacker") end)

    -- AIMBOT CONTENT
    local aimbotToggleLabel = Instance.new("TextLabel")
    aimbotToggleLabel.Size = UDim2.new(0.8, 0, 0, 30)
    aimbotToggleLabel.Position = UDim2.new(0.05, 0, 0.05, 0)
    aimbotToggleLabel.BackgroundTransparency = 1
    aimbotToggleLabel.Text = "Ativar AimBot"
    aimbotToggleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    aimbotToggleLabel.TextXAlignment = Enum.TextXAlignment.Left
    aimbotToggleLabel.Parent = aimbotContent

    local aimbotToggle = Instance.new("TextButton")
    aimbotToggle.Size = UDim2.new(0, 60, 0, 30)
    aimbotToggle.Position = UDim2.new(0.75, 0, 0.05, 0)
    aimbotToggle.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    aimbotToggle.Text = "OFF"
    aimbotToggle.TextColor3 = Color3.fromRGB(255, 255, 255)
    aimbotToggle.Parent = aimbotContent

    local fovLabel = Instance.new("TextLabel")
    fovLabel.Size = UDim2.new(0.8, 0, 0, 30)
    fovLabel.Position = UDim2.new(0.05, 0, 0.15, 0)
    fovLabel.BackgroundTransparency = 1
    fovLabel.Text = "Tamanho do FOV: 200"
    fovLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    fovLabel.TextXAlignment = Enum.TextXAlignment.Left
    fovLabel.Parent = aimbotContent

    local fovSlider = Instance.new("TextButton")
    fovSlider.Size = UDim2.new(0.8, 0, 0, 30)
    fovSlider.Position = UDim2.new(0.05, 0, 0.23, 0)
    fovSlider.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    fovSlider.Text = ""
    fovSlider.Parent = aimbotContent

    -- FOV Slider Logic
    local function updateFOVSize(input)
        local relX = (input.Position.X - fovSlider.AbsolutePosition.X) / fovSlider.AbsoluteSize.X
        fovSize = math.clamp(math.floor(relX * 500), 50, 500)
        fovLabel.Text = "Tamanho do FOV: " .. fovSize
        if fovCircle then
            fovCircle.Radius = fovSize
        end
    end

    fovSlider.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            updateFOVSize(input)
        end
    end)

    fovSlider.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            if userInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton1) then
                updateFOVSize(input)
            end
        end
    end)

    -- Criar/atualizar círculo FOV
    local function createFOV()
        if fovCircle then 
            pcall(function() fovCircle:Remove() end)
        end
        fovCircle = Drawing.new("Circle")
        fovCircle.Visible = false
        fovCircle.Radius = fovSize
        fovCircle.Color = Color3.fromRGB(255, 255, 255)
        fovCircle.Thickness = 2
        fovCircle.Filled = false
        fovCircle.Position = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)
    end

    -- AIMBOT FUNCIONANDO CORRETAMENTE
    local function aimbotFunction()
        if not aimbotEnabled then 
            if fovCircle then fovCircle.Visible = false end
            return 
        end
        
        if fovCircle then
            fovCircle.Position = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)
            fovCircle.Visible = true
        end
        
        local closestPlayer = nil
        local shortestDistance = fovSize
        local cameraPos = camera.CFrame.Position
        local screenCenter = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)
        
        for _, plr in ipairs(players:GetPlayers()) do
            if plr ~= player and plr.Character then
                local head = plr.Character:FindFirstChild("Head")
                local humanoid = plr.Character:FindFirstChild("Humanoid")
                
                if head and humanoid and humanoid.Health > 0 then
                    local screenPos, onScreen = camera:WorldToViewportPoint(head.Position)
                    
                    if onScreen then
                        local screenPoint = Vector2.new(screenPos.X, screenPos.Y)
                        local distanceFromCenter = (screenPoint - screenCenter).Magnitude
                        
                        -- Verificar se está dentro do FOV
                        if distanceFromCenter <= fovSize then
                            -- Verificar se há linha de visão direta (sem paredes)
                            local raycastParams = RaycastParams.new()
                            raycastParams.FilterType = Enum.RaycastFilterType.Blacklist
                            raycastParams.FilterDescendantsInstances = {player.Character, plr.Character}
                            
                            local direction = (head.Position - cameraPos).Unit
                            local distance = (head.Position - cameraPos).Magnitude
                            local raycastResult = workspace:Raycast(cameraPos, direction * distance, raycastParams)
                            
                            -- Se não houver obstáculo ou o obstáculo for 
