local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/apeludo999/GgScripts/refs/heads/main/interfaceAETHER')))()
local Window = OrionLib:MakeWindow({
    Name = "Aether Hub | V1.1", 
    HidePremium = false, 
    SaveConfig = true, 
    ConfigFolder = "AetherHubConfigs",
    IntroEnabled = true,
    IntroText = "ZKM Team",
    IntroIcon = "rbxassetid://4483345998",
    Icon = "rbxassetid://4483345998",
    CloseCallback = function() 
        print("Aether Hub fechado")
    end
})

OrionLib:MakeNotification({
    Name = "Script",
    Content = "Aether Hub carregado com sucesso",
    Image = "rbxassetid://4483345998",
    Time = 5
})

-- VariÃ¡veis
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local infiniteJumpEnabled = false
local noclipEnabled = false
local loopColorEnabled = false
local loopCurtainsEnabled = false
local loopGarageEnabled = false
local selectedPlayerName = ""
local selectedHouseNumber = 1
local repeatName = "DefaultName"
local repeatBio = "DefaultBio"
local rainbowNameEnabled = false
local rainbowBioEnabled = false

-- FunÃ§Ãµes
local function getPlayerCount()
    return #Players:GetPlayers()
end

local function getGameName()
    return game:GetService("MarketplaceService"):GetProductInfo(game.PlaceId).Name
end

local function getExecutor()
    return identifyexecutor and identifyexecutor() or "Unknown"
end

local function toggleInfiniteJump(value)
    infiniteJumpEnabled = value
end

UserInputService.JumpRequest:Connect(function()
    if infiniteJumpEnabled then
        local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid:ChangeState("Jumping")
        end
    end
end)

local function toggleNoclip(value)
    noclipEnabled = value
    if noclipEnabled then
        RunService.Stepped:Connect(function()
            if noclipEnabled and LocalPlayer.Character then
                for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        end)
    end
end

local function loopColor()
    while loopColorEnabled do
        local event = game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sHous1e")
        if event then
            event:FireServer("ColorPickHouse", Color3.fromRGB(math.random(0, 255), math.random(0, 255), math.random(0, 255)))
        end
        task.wait(0.5)
    end
end

local function loopCurtains()
    while loopCurtainsEnabled do
        local event = game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sHous1e")
        if event then
            event:FireServer("Curtains")
        end
        task.wait(1)
    end
end

local function loopGarage()
    while loopGarageEnabled do
        local event = game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sHous1e")
        if event then
            event:FireServer("GarageDoor")
        end
        task.wait(1)
    end
end

local function changeRPNameColor(color)
    local args = {
        [1] = "PickingRPNameColor",
        [2] = color
    }
    game:GetService("ReplicatedStorage").RE:FindFirstChild("1RPNam1eColo1r"):FireServer(unpack(args))
end

local function changeRPBioColor(color)
    local args = {
        [1] = "PickingRPBioColor",
        [2] = color
    }
    game:GetService("ReplicatedStorage").RE:FindFirstChild("1RPNam1eColo1r"):FireServer(unpack(args))
end

-- FunÃ§Ãµes para loops de efeito arco-Ã­ris
local function rainbowNameLoop()
    while rainbowNameEnabled do
        for i = 0, 1, 0.01 do
            if not rainbowNameEnabled then break end
            local color = Color3.fromHSV(i, 1, 1)
            changeRPNameColor(color)
            task.wait(0.2)
        end
    end
end

local function rainbowBioLoop()
    while rainbowBioEnabled do
        for i = 0, 1, 0.01 do
            if not rainbowBioEnabled then break end
            local color = Color3.fromHSV(i, 1, 1)
            changeRPBioColor(color)
            task.wait(0.2)
        end
    end
end

-- Tabs e SeÃ§Ãµes
local InfoTab = Window:MakeTab({
    Name = "Info",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

InfoTab:AddSection({
    Name = "Esta Hub foi inspirada na RedzHub"
})

InfoTab:AddSection({
    Name = "Obrigado por usar nosso script :)"
})

local CreditosSection = InfoTab:AddSection({ Name = "CrÃ©ditos" })

CreditosSection:AddLabel("Criadores - Script: Silent_xit | ruby09758")

local MainTab = Window:MakeTab({
    Name = "Main",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

local HubSection = MainTab:AddSection({
    Name = "<Settings - Hub>"
})

MainTab:AddLabel("Players in Map: " .. getPlayerCount())
MainTab:AddLabel("Game Name: " .. getGameName())
MainTab:AddLabel("Executor: " .. getExecutor())

local PlayerSection = MainTab:AddSection({
    Name = "<Settings - Player>"
})

MainTab:AddTextbox({
    Name = "JumpPower",
    Default = "50",  
    TextDisappear = true,
    Callback = function(value)
        local jumpPower = tonumber(value)
        if jumpPower and LocalPlayer.Character then
            LocalPlayer.Character:FindFirstChildOfClass("Humanoid").JumpPower = jumpPower
        else
            warn("Valor invÃ¡lido para JumpPower.")
        end
    end
})

MainTab:AddTextbox({
    Name = "WalkSpeed",
    Default = "16",  
    TextDisappear = true,
    Callback = function(value)
        local walkSpeed = tonumber(value)
        if walkSpeed and LocalPlayer.Character then
            LocalPlayer.Character:FindFirstChildOfClass("Humanoid").WalkSpeed = walkSpeed
        else
            warn("Valor invÃ¡lido para WalkSpeed.")
        end
    end
})

MainTab:AddTextbox({
    Name = "Gravity",
    Default = "196.2",  
    TextDisappear = true,
    Callback = function(value)
        local gravity = tonumber(value)
        if gravity then
            game:GetService("Workspace").Gravity = gravity
        else
            warn("Valor invÃ¡lido para Gravity.")
        end
    end
})

MainTab:AddToggle({
    Name = "Infinite Jump",
    Default = false,
    Callback = toggleInfiniteJump
})

MainTab:AddToggle({
    Name = "Noclip",
    Default = false,
    Callback = toggleNoclip
})

-- PlayersTab
local PlayersTab = Window:MakeTab({
    Name = "Players",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

local TeleportSection = PlayersTab:AddSection({
    Name = "Teleport/View Settings"
})

local function updatePlayerList()
    local playerList = {}
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            table.insert(playerList, player.Name)
        end
    end
    return playerList
end

PlayersTab:AddDropdown({
    Name = "Select Player",
    Default = "",
    Options = updatePlayerList(),
    Callback = function(value)
        selectedPlayerName = value
    end
})

local function teleportToPlayer(playerName)
    local targetPlayer = Players:FindFirstChild(playerName)
    if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local targetRootPart = targetPlayer.Character.HumanoidRootPart
        LocalPlayer.Character:SetPrimaryPartCFrame(targetRootPart.CFrame)
    else
        warn("Jogador nÃ£o encontrado ou sem personagem vÃ¡lido!")
    end
end

PlayersTab:AddButton({
    Name = "Teleport to Player",
    Callback = function()
        if selectedPlayerName then
            teleportToPlayer(selectedPlayerName)
        else
            warn("Selecione um jogador primeiro!")
        end
    end
})

local function teleportToRandomPlayer()
    local players = updatePlayerList()
    if #players > 0 then
        local randomPlayer = players[math.random(1, #players)]
        teleportToPlayer(randomPlayer)
    else
        warn("Nenhum jogador disponÃ­vel para teleportar!")
    end
end

PlayersTab:AddButton({
    Name = "Teleport to Random Player",
    Callback = teleportToRandomPlayer
})

local function startSpectate(targetPlayer)
    if targetPlayer and targetPlayer.Character then
        workspace.CurrentCamera.CameraSubject = targetPlayer.Character:FindFirstChildOfClass("Humanoid")
    else
        warn("Jogador nÃ£o encontrado ou sem personagem vÃ¡lido!")
    end
end

local function stopSpectate()
    workspace.CurrentCamera.CameraSubject = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
end

PlayersTab:AddToggle({
    Name = "Spectate Player",
    Default = false,
    Callback = function(enabled)
        if enabled then
            local targetPlayer = Players:FindFirstChild(selectedPlayerName)
            startSpectate(targetPlayer)
        else
            stopSpectate()
        end
    end
})

-- HouseTab
local HouseTab = Window:MakeTab({
    Name = "House",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

local HouseSection = HouseTab:AddSection({
    Name = "Pegar PermissÃ£o"
})

local houseOptions = {}
for i = 1, 22 do
    table.insert(houseOptions, "House " .. i)
end

HouseSection:AddDropdown({
    Name = "Select House ðŸ¡",
    Default = "House 1",
    Options = houseOptions,
    Callback = function(value)
        selectedHouseNumber = tonumber(value:match("%d+")) or 1
    end
})

HouseSection:AddButton({
    Name = "Pegar PermissÃ£o",
    Callback = function()
        local event = game:GetService("ReplicatedStorage").RE:FindFirstChild("1Playe1rTrigge1rEven1t")
        if event then
            event:FireServer("GivePermissionLoopToServer", LocalPlayer, selectedHouseNumber)
        else
            warn("Evento de permissÃ£o nÃ£o encontrado!")
        end
    end
})

local LoopSection = HouseTab:AddSection({
    Name = "House Loops"
})

LoopSection:AddToggle({
    Name = "Loop Color (Casa)",
    Default = false,
    Callback = function(value)
        loopColorEnabled = value
        if value then
            spawn(loopColor)
        end
    end
})

LoopSection:AddToggle({
    Name = "Loop Cortinas",
    Default = false,
    Callback = function(value)
        loopCurtainsEnabled = value
        if value then
            spawn(loopCurtains)
        end
    end
})

LoopSection:AddToggle({
    Name = "Loop Garagem",
    Default = false,
    Callback = function(value)
        loopGarageEnabled = value
        if value then
            spawn(loopGarage)
        end
    end
})

local AvatarTab = Window:MakeTab({
    Name = "Avatar",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

local RPSection = AvatarTab:AddSection({
    Name = "Repeat"
})

RPSection:AddTextbox({
    Name = "Repeat Name",
    Default = "DefaultName",  
    TextDisappear = false,
    Callback = function(value)
        repeatName = value
        local args = {
            [1] = "PickingRPName",
            [2] = repeatName
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1RPNam1eColo1r"):FireServer(unpack(args))
    end
})


RPSection:AddTextbox({
    Name = "Repeat Bio",
    Default = "DefaultBio",  
    TextDisappear = false,
    Callback = function(value)
        repeatBio = value
        local args = {
            [1] = "PickingRPBio",
            [2] = repeatBio
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1RPNam1eColo1r"):FireServer(unpack(args))
    end
})

local RPSection = AvatarTab:AddSection({
    Name = "Loop color"
})

RPSection:AddToggle({
    Name = "Loop Name<Raimbow>",
    Default = false,
    Callback = function(value)
        rainbowNameEnabled = value
        if value then
            spawn(rainbowNameLoop)
        end
    end
})


RPSection:AddToggle({
    Name = "Loop Bio<Rainbow>",
    Default = false,
    Callback = function(value)
        rainbowBioEnabled = value
        if value then
            spawn(rainbowBioLoop)
        end
    end
})

OrionLib:Init()


