-- FOUR HUB v3.0 - com aba INFINITE adicionada + emojis
-- Integrado: funcionalidades originais + aba "Infinite" (Castelo, Deserto, BossRush) com emojis

local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local Remote = game:GetService("ReplicatedStorage"):WaitForChild("BridgeNet2"):WaitForChild("dataRemoteEvent")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Player = Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")

---------------------------------------------------------------------
-- INFINITE: Funções e checkpoints (integrado na GUI como aba "Infinite")
---------------------------------------------------------------------
local function StartRun(eventName, checkpoint)
    Remote:FireServer({
        {
            Event = eventName,
            Action = "Start",
            Dungeon = 4286839868,
            Check = tostring(checkpoint)
        },
        "\016"
    })
end

local function CreateRoom(eventName)
    Remote:FireServer({
        {
            Event = eventName,
            Action = "Create"
        },
        "\016"
    })
end

local function StartBoss(diff)
    Remote:FireServer({
        {
            Event = "BossRushAction",
            Action = "Start",
            Dungeon = 4286839868,
            Diff = diff
        },
        "\016"
    })
end

local function CreateBoss()
    Remote:FireServer({
        {
            Event = "BossRushAction",
            Action = "Create"
        },
        "\016"
    })
end

local checkpointsCastelo = {30,60,90,120,150,180,210,240,270,300,330,360,390,420,450}
local checkpointsDeserto = {30,60,90,120,150,180,210,240,270,300,330,360,390,420,450,480,510,540,570,600,630,660,690}
local bossList = { "Easy", "Normal", "Hard", "Insane", "Ultra" }

---------------------------------------------------------------------
-- (Restante do código FOUR HUB original)
---------------------------------------------------------------------

--==================== FUNÇÕES DE REMOTE ====================
local function ComprarItem(shop, item)
    Remote:FireServer({
        {
            Shop = shop, Action = "Buy", Amount = 1, Item = item, Event = "ItemShopAction", Rank = 1
        }, "\016"
    })
end

-- Agora aceita um parâmetro 'upgrader' para especificar qual Upgrader usar
local function FazerUpgrade(target, upgrader)
    Remote:FireServer({
        {
            Target = target, Event = "UpgradeAction", Action = "Upgrade", Upgrade = upgrader
        }, "\016"
    })
end

local function FazerGacha(name)
    Remote:FireServer({
        {
            GachaName = name, Event = "Gacha", Action = "Roll"
        }, "\016"
    })
end

local function CreateDungeon()
    Remote:FireServer({
        {
            Event = "DungeonAction", Action = "Create"
        }, "\016"
    })
end

local function StartDungeon()
    Remote:FireServer({
        {
            Dungeon = 4286839868, Event = "DungeonAction", Action = "Start"
        }, "\016"
    })
end

--==================== TOGGLES MASTER ====================
local Toggles = {
    Exp = false, Core = false, Token = false, MoreRoom = false,
    DungeonRank = false, DoubleDungeon = false,
    ["Grass"] = false, ["Brum"] = false, ["FaceHeal"] = false, ["Xz"] = false, ["Mage"] = false, ["Shield"] = false,
    ["Slayer"] = false, ["Ghoul"] = false, ["Eminance"] = false, ["Lucky"] = false, ["Hunters"] = false, ["Monarch"] = false,
    ["Mori"] = false, ["Deadly"] = false, ["Sao"] = false, ["Zero"] = false,
    AutoUpgradeGacha1 = false, AutoUpgradeGacha2 = false,
    AutoUpgradeRunes = false, AutoUpgradeResource = false, AutoUpgradeKey = false,
    AutoUpgradeBoosts = false, AutoUpgradeCores = false, AutoUpgradeTokens = false,
    AutoDungeonSmart = false, AutoDungeonINF = false, AutoFarmCompleto = false,
    NoClip = false, RemoveTeleportUI = false, AntiAFK = false, AutoExecute = false,
    FPSBooster = false, StatsDashboard = false
}

local GachaGUI = {"Grass","Brum","FaceHeal","Xz","Mage","Shield","Slayer","Ghoul","Eminance","Lucky","Hunters","Monarch","Mori","Deadly","Sao","Zero"}
local GachaRemoteNames = {
    "NarutoClan","PirateCrew","BleachShikais","HeroRanksOPM","FrierenPartys","ShieldHeroArmaments","DemonMoons",
    "KaguneTypes","EminenceRelics","BlackCloverGrimoires","SoloLevelingRanks","SoloLevelingRelics","JoestarBloodline",
    "SinCrests","SaoGuilds","ReZeroWitchFactors"
}

-- inicializa toggles para cada gacha nomeado
for i, guiName in ipairs(GachaGUI) do
    Toggles[guiName] = Toggles[guiName] or false
end

--==================== UPGRADE GACHA (Shadow / Weapons) ====================
local ShadowGUI = {"Solo","Nipon","Dragon","Kindama","Nen","Hurricane","Cursed","Kaiju","Zero"}
local WeaponsGUI = {"Solo","Nipon","Dragon","Kindama","Nen","Hurricane","Cursed","Kaiju","Zero"}

local ShadowRemote = {
    Solo = "LevelingUpgrader",
    Nipon = "ChainsawUpgrader",
    Dragon = "DragonCityUpgrader",
    Kindama = "KindamaCityUpgrader",
    Nen = "NenCityUpgrader",
    Hurricane = "HurricaneUpgrader",
    Cursed = "CursedHighUpgrader",
    Kaiju = "KaijuUpgrader",
    Zero = "ReZeroUpgrader"
}

local WeaponsRemote = {
    Solo = "LevelingUpgrader",
    Nipon = "ChainsawUpgrader",
    Dragon = "DragonCityUpgrader",
    Kindama = "KindamaCityUpgrader",
    Nen = "NenCityUpgrader",
    Hurricane = "HurricaneUpgrader",
    Cursed = "CursedHighUpgrader",
    Kaiju = "KaijuUpgrader",
    Zero = "ReZeroUpgrader"
}

for i, name in ipairs(ShadowGUI) do
    Toggles[name.."Shadow"] = false
end
for i, name in ipairs(WeaponsGUI) do
    Toggles[name.."Weapons"] = false
end

--==================== FPS BOOSTER MOBILE ====================
local FPSBoosterActive = false

local function BoostFPS()
    if FPSBoosterActive then return end
    FPSBoosterActive = true
    
    task.spawn(function()
        pcall(function()
            game.Lighting.GlobalShadows = false
            game.Lighting.Brightness = 2
            game.Lighting.Ambient = Color3.fromRGB(200, 200, 200)
        end)
        
        for _, v in pairs(workspace:GetDescendants()) do
            pcall(function()
                if v:IsA("ParticleEmitter") then v.Enabled = false end
                if v:IsA("Smoke") then v.Enabled = false end
                if v:IsA("Fire") then v.Enabled = false end
            end)
        end
        
        for _, v in pairs(workspace:GetDescendants()) do
            pcall(function()
                if v:IsA("Decal") then v:Destroy() end
            end)
        end
        
        for _, v in pairs(workspace:GetDescendants()) do
            pcall(function()
                if v:IsA("BasePart") then
                    v.Material = Enum.Material.Plastic
                    v.CanCollide = true
                end
            end)
        end
        
        pcall(function()
            game.Lighting.EnvironmentDiffuseScale = 0
            game.Lighting.EnvironmentSpecularScale = 0
        end)
    end)
    
    Fluent:Notify({Title = "FPS Booster", Content = "✅ Mobile otimizado! Lag removido!", Duration = 4})
end

--==================== STATS DASHBOARD ====================
local StatsActive = false
local StatsUI = nil
local StartTime = tick()
local InitialCoins = 0
local InitialExp = 0

local function CreateStatsDashboard()
    local PlayerGui = Player:WaitForChild("PlayerGui")
    
    local oldDash = PlayerGui:FindFirstChild("StatsDashboard")
    if oldDash then oldDash:Destroy() end
    
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "StatsDashboard"
    screenGui.ResetOnSpawn = false
    screenGui.Parent = PlayerGui
    
    local mainFrame = Instance.new("Frame")
    mainFrame.Name = "MainFrame"
    mainFrame.Size = UDim2.new(0, 280, 0, 180)
    mainFrame.Position = UDim2.new(0, 10, 0, 100)
    mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    mainFrame.BorderSizePixel = 2
    mainFrame.BorderColor3 = Color3.fromRGB(100, 200, 255)
    mainFrame.Parent = screenGui
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = mainFrame
    
    local title = Instance.new("TextLabel")
    title.Name = "Title"
    title.Size = UDim2.new(1, 0, 0, 30)
    title.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    title.BorderSizePixel = 0
    title.Font = Enum.Font.GothamBold
    title.Text = "📊 STATS"
    title.TextColor3 = Color3.fromRGB(100, 200, 255)
    title.TextSize = 16
    title.Parent = mainFrame
    
    local titleCorner = Instance.new("UICorner")
    titleCorner.CornerRadius = UDim.new(0, 8)
    titleCorner.Parent = title
    
    local stats = Instance.new("TextLabel")
    stats.Name = "Stats"
    stats.Size = UDim2.new(1, -10, 1, -40)
    stats.Position = UDim2.new(0, 5, 0, 35)
    stats.BackgroundTransparency = 1
    stats.Font = Enum.Font.Gotham
    stats.TextColor3 = Color3.fromRGB(200, 200, 200)
    stats.TextSize = 12
    stats.TextXAlignment = Enum.TextXAlignment.Left
    stats.TextYAlignment = Enum.TextYAlignment.Top
    stats.Text = "Tempo: 0m\nEXP/h: 0\nCoins/h: 0"
    stats.Parent = mainFrame
    
    StatsUI = {Frame = mainFrame, Stats = stats, ScreenGui = screenGui}
    return screenGui
end

local function UpdateStats()
    if not StatsActive or not StatsUI then return end
    
    local elapsedTime = tick() - StartTime
    local minutes = math.floor(elapsedTime / 60)
    
    local currentCoins = 0
    local currentExp = 0
    
    pcall(function()
        local leaderstats = Player:FindFirstChild("leaderstats")
        if leaderstats then
            if leaderstats:FindFirstChild("Coins") then
                currentCoins = leaderstats.Coins.Value or 0
            end
            if leaderstats:FindFirstChild("Exp") then
                currentExp = leaderstats.Exp.Value or 0
            end
            if leaderstats:FindFirstChild("Experience") then
                currentExp = leaderstats.Experience.Value or 0
            end
        end
    end)
    
    local coinsPerHour = 0
    local expPerHour = 0
    
    if elapsedTime > 60 then
        coinsPerHour = math.floor((currentCoins - InitialCoins) / (elapsedTime / 3600))
        expPerHour = math.floor((currentExp - InitialExp) / (elapsedTime / 3600))
    end
    
    if StatsUI and StatsUI.Stats then
        StatsUI.Stats.Text = string.format(
            "⏱️ Tempo: %dm\n💰 Coins/h: %d\n⭐ EXP/h: %d\n\n💵 Total: %d\n✨ EXP: %d",
            minutes, coinsPerHour, expPerHour, currentCoins, currentExp
        )
    end
end

--==================== WEBHOOK LOGGER ====================
local WEBHOOK_URL = ""
local WebhookActive = false

local function SendWebhook(title, message, color)
    if WEBHOOK_URL == "" or WEBHOOK_URL == nil then 
        print("[WEBHOOK] URL não configurada!")
        Fluent:Notify({Title = "Webhook", Content = "❌ URL não configurada!", Duration = 3})
        return 
    end
    
    task.spawn(function()
        pcall(function()
            local HttpService = game:GetService("HttpService")
            local data = {
                embeds = {{
                    title = title,
                    description = message,
                    color = color or 3447003,
                    timestamp = os.date("!%Y-%m-%dT%H:%M:%SZ")
                }}
            }
            
            local json = HttpService:JSONEncode(data)
            HttpService:PostAsync(WEBHOOK_URL, json, Enum.HttpContentType.ApplicationJson)
            print("[WEBHOOK] ✅ Mensagem enviada com sucesso!")
        end)
    end)
end

local function LogLoot(itemName, quantity, rarity)
    SendWebhook("🎁 LOOT ENCONTRADO", string.format("**%s** x%d\n*Raridade: %s*", itemName, quantity, rarity), 16776960)
end

local function LogBossKill(bossName, difficulty)
    SendWebhook("💀 BOSS DERROTADO", string.format("**%s** - %s", bossName, difficulty), 16711680)
end

local function LogAchievement(achievementName)
    SendWebhook("🏆 ACHIEVEMENT", achievementName, 65280)
end

--==================== SISTEMA AUTO DUNGEON ====================
local AutoDungeonSmart = {Active = false, Loop = nil, CheckInterval = 0.3}

local function CountNPCs()
    local serverFolder = workspace:FindFirstChild("__Main")
    if not serverFolder then return 0 end
    serverFolder = serverFolder:FindFirstChild("__Enemies")
    if not serverFolder then return 0 end
    serverFolder = serverFolder:FindFirstChild("Server")
    if not serverFolder then return 0 end
    local count = 0
    for _, child in ipairs(serverFolder:GetChildren()) do
        if child:IsA("BasePart") or child:IsA("MeshPart") or child:IsA("Model") then
            count = count + 1
        end
    end
    return count
end

local function PortalExists()
    local worldFolder = workspace:FindFirstChild("__Main")
    if not worldFolder then return false end
    worldFolder = worldFolder:FindFirstChild("__World")
    if not worldFolder then return false end
    for _, model in ipairs(worldFolder:GetDescendants()) do
        if model.Name == "FirePortal" or model.Name == "DungeonSpawn" or model.Name == "DoubleDungeonSpawn" then
            return true
        end
    end
    return false
end

local function WaitForNPCsToSpawn()
    local startTime = tick()
    local maxWait = 8
    while tick() - startTime < maxWait do
        if CountNPCs() > 0 then
            return true
        end
        task.wait(0.3)
    end
    return false
end

local function SmartDungeonLoop()
    local zeroNPCTime = 0
    local hadNPCs = false
    while AutoDungeonSmart.Active do
        local currentNPCs = CountNPCs()
        local hasPortal = PortalExists()
        if currentNPCs > 0 then
            hadNPCs = true
            zeroNPCTime = 0
        end
        if currentNPCs == 0 and hasPortal and hadNPCs then
            CreateDungeon()
            task.wait(0.5)
            StartDungeon()
            WaitForNPCsToSpawn()
            hadNPCs = false
            zeroNPCTime = 0
        elseif currentNPCs == 0 and not hasPortal and hadNPCs then
            zeroNPCTime = zeroNPCTime + AutoDungeonSmart.CheckInterval
            if zeroNPCTime >= 6 then
                CreateDungeon()
                task.wait(0.5)
                StartDungeon()
                WaitForNPCsToSpawn()
                hadNPCs = false
                zeroNPCTime = 0
            end
        end
        task.wait(AutoDungeonSmart.CheckInterval)
    end
end

function AutoDungeonSmart:Start()
    if self.Active then return end
    self.Active = true
    self.Loop = task.spawn(SmartDungeonLoop)
    Fluent:Notify({Title = "Auto Dungeon", Content = "✅ Ativado!", Duration = 3})
end

function AutoDungeonSmart:Stop()
    if not self.Active then return end
    self.Active = false
    if self.Loop then task.cancel(self.Loop) end
    Fluent:Notify({Title = "Auto Dungeon", Content = "❌ Desativado!", Duration = 3})
end

--==================== SISTEMA AUTO FARM ====================
local AutoFarmSystem = {Active = false, MainLoop = nil}
local CONFIG = {
    ISLANDS = {"Solo2World", "Solo3World", "TokyoWorld", "KaijuWorld"},
    TP_DELAY = 0.5,
    CHECK_DELAY = 0.5,
    GIGANTE_CFRAME = CFrame.new(
    5474.10059, 34.8206902, 2421.89062,
    0.923500896, 0.375369132, -0.0790196285,
    -0.310007632, 0.851654828, 0.422586441,
    0.225923359, -0.365762293, 0.902871311
)
}
local ClientEnemies = workspace.__Main.__Enemies:WaitForChild("Client")
local SpawnsFolder = workspace.__Extra.__Spawns

Player.CharacterAdded:Connect(function(newChar)
    Character = newChar
    HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
end)

local function UpdateCharacter()
    Character = Player.Character or Player.CharacterAdded:Wait()
    HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
end

local function TeleportToIslandSpawn(islandName)
    local spawnPoint = SpawnsFolder:FindFirstChild(islandName)
    if spawnPoint then
        HumanoidRootPart.CFrame = spawnPoint.CFrame + Vector3.new(0, 3, 0)
        return true
    end
    return false
end

local function GetIslandNPCs(islandName)
    local enemies = {}
    local islandFolder = ClientEnemies:FindFirstChild(islandName)
    if islandFolder then
        for _, npc in ipairs(islandFolder:GetChildren()) do
            if npc:IsA("Model") and npc:FindFirstChild("HumanoidRootPart") then
                table.insert(enemies, npc)
            end
        end
    end
    return enemies
end

local function GetAllClientNPCs()
    local enemies = {}
    for _, islandFolder in ipairs(ClientEnemies:GetChildren()) do
        if islandFolder:IsA("Folder") then
            for _, npc in ipairs(islandFolder:GetChildren()) do
                if npc:IsA("Model") and npc:FindFirstChild("HumanoidRootPart") then
                    table.insert(enemies, npc)
                end
            end
        end
    end
    return enemies
end

local function KillNPC(npc)
    if not npc or not npc.Parent then return end
    local npcRoot = npc:FindFirstChild("HumanoidRootPart")
    if npcRoot then
        HumanoidRootPart.CFrame = CFrame.new(npcRoot.Position + Vector3.new(0, 3, 0))
    end
end

local function FarmIsland(islandName)
    if not AutoFarmSystem.Active then return end
    if not TeleportToIslandSpawn(islandName) then return false end
    task.wait(1)
    while AutoFarmSystem.Active do
        local npcs = GetIslandNPCs(islandName)
        if #npcs == 0 then break end
        for _, npc in ipairs(npcs) do
            if not AutoFarmSystem.Active then return false end
            KillNPC(npc)
            task.wait(CONFIG.TP_DELAY)
        end
        task.wait(CONFIG.CHECK_DELAY)
    end
    return true
end

local function FarmAllClientNPCs()
    HumanoidRootPart.CFrame = CONFIG.GIGANTE_CFRAME
    task.wait(1)
    while AutoFarmSystem.Active do
        local allNPCs = GetAllClientNPCs()
        if #allNPCs == 0 then break end
        for _, npc in ipairs(allNPCs) do
            if not AutoFarmSystem.Active then return end
            KillNPC(npc)
            task.wait(CONFIG.TP_DELAY)
        end
        task.wait(CONFIG.CHECK_DELAY)
    end
end

local function MainFarmLoop()
    while AutoFarmSystem.Active do
        for _, islandName in ipairs(CONFIG.ISLANDS) do
            if not AutoFarmSystem.Active then break end
            FarmIsland(islandName)
            task.wait(1)
        end
        if AutoFarmSystem.Active then
            FarmAllClientNPCs()
        end
        task.wait(3)
    end
end

function AutoFarmSystem:Start()
    if self.Active then return end
    UpdateCharacter()
    self.Active = true
    self.MainLoop = task.spawn(function() MainFarmLoop() end)
    Fluent:Notify({Title = "Auto Farm", Content = "✅ Sistema ativado!", Duration = 4})
end

function AutoFarmSystem:Stop()
    if not self.Active then return end
    self.Active = false
    if self.MainLoop then task.cancel(self.MainLoop) end
    Fluent:Notify({Title = "Auto Farm", Content = "❌ Desativado!", Duration = 3})
end

--==================== SISTEMA TELEPORTE ====================
local TeleportSystem = {Active = false, UI = nil, Connections = {}}

function TeleportSystem:Stop()
    if not self.Active then return end
    for _, connection in pairs(self.Connections) do
        if connection then pcall(function() connection:Disconnect() end) end
    end
    self.Connections = {}
    if self.UI and self.UI.Button then
        pcall(function() self.UI.Button.Parent:Destroy() end)
    end
    self.UI = nil
    self.Active = false
    Fluent:Notify({Title = "Auto Dungeon/INF", Content = "❌ Desativado!", Duration = 3})
end

function TeleportSystem:Initialize()
    if self.Active then return end
    self.Active = true
    local player = Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
    local CONFIG_TP = {TELEPORT_DISTANCE = 10, TELEPORT_OFFSET = Vector3.new(0, 2, 0), UPDATE_INTERVAL = 0.05, HIGHLIGHT_COLOR = Color3.fromRGB(255, 140, 0), OUTLINE_COLOR = Color3.fromRGB(255, 255, 255), PORTAL_CHECK_INTERVAL = 0.5, PROXIMITY_HOLD_TIME = 0.5}
    local serverFolder = workspace:WaitForChild("__Main"):WaitForChild("__Enemies"):WaitForChild("Server")
    local worldFolder = workspace:WaitForChild("__Main"):WaitForChild("__World")
    local currentTargetIndex = 0
    local highlightCache = {}
    local partsCache = {}
    local lastUpdate = 0
    local isTeleporting = false
    local allEnemiesDead = false
    local isSystemActive = true

    local function createMobileUI()
        local playerGui = player:WaitForChild("PlayerGui")
        local oldUI = playerGui:FindFirstChild("TeleportSystemUI")
        if oldUI then oldUI:Destroy() end
        task.wait(0.1)
        local screenGui = Instance.new("ScreenGui")
        screenGui.Name = "TeleportSystemUI"
        screenGui.ResetOnSpawn = false
        screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
        screenGui.IgnoreGuiInset = false
        screenGui.DisplayOrder = 100
        local mainButton = Instance.new("TextButton")
        mainButton.Name = "ToggleButton"
        mainButton.Size = UDim2.new(0, 100, 0, 45)
        mainButton.Position = UDim2.new(1, -110, 0, 10)
        mainButton.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
        mainButton.BorderSizePixel = 3
        mainButton.BorderColor3 = Color3.fromRGB(255, 255, 255)
        mainButton.Font = Enum.Font.GothamBold
        mainButton.Text = "ATIVO"
        mainButton.TextColor3 = Color3.fromRGB(255, 255, 255)
        mainButton.TextSize = 16
        mainButton.TextScaled = true
        mainButton.AutoButtonColor = true
        mainButton.ZIndex = 101
        mainButton.Active = true
        mainButton.Parent = screenGui
        local textPadding = Instance.new("UIPadding")
        textPadding.PaddingLeft = UDim.new(0, 5)
        textPadding.PaddingRight = UDim.new(0, 5)
        textPadding.PaddingTop = UDim.new(0, 5)
        textPadding.PaddingBottom = UDim.new(0, 5)
        textPadding.Parent = mainButton
        local corner = Instance.new("UICorner")
        corner.CornerRadius = UDim.new(0, 8)
        corner.Parent = mainButton
        local statusLabel = Instance.new("TextLabel")
        statusLabel.Name = "StatusLabel"
        statusLabel.Size = UDim2.new(0, 100, 0, 25)
        statusLabel.Position = UDim2.new(1, -110, 0, 60)
        statusLabel.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
        statusLabel.BorderSizePixel = 2
        statusLabel.BorderColor3 = Color3.fromRGB(255, 255, 255)
        statusLabel.Font = Enum.Font.Gotham
        statusLabel.Text = "Aguardando..."
        statusLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
        statusLabel.TextSize = 12
        statusLabel.TextScaled = true
        statusLabel.ZIndex = 101
        statusLabel.Parent = screenGui
        local statusPadding = Instance.new("UIPadding")
        statusPadding.PaddingLeft = UDim.new(0, 3)
        statusPadding.PaddingRight = UDim.new(0, 3)
        statusPadding.Parent = statusLabel
        local statusCorner = Instance.new("UICorner")
        statusCorner.CornerRadius = UDim.new(0, 6)
        statusCorner.Parent = statusLabel
        screenGui.Parent = playerGui
        return mainButton, statusLabel
    end

    task.wait(0.5)
    local toggleButton, statusLabel = createMobileUI()
    self.UI = {Button = toggleButton, Status = statusLabel}

    local function updateButtonAppearance()
        if not toggleButton then return end
        if isSystemActive then
            toggleButton.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
            toggleButton.Text = "ATIVO"
        else
            toggleButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
            toggleButton.Text = "PAUSADO"
        end
    end

    local function updateStatusLabel(text)
        if statusLabel then statusLabel.Text = text end
    end

    local function toggleSystem()
        isSystemActive = not isSystemActive
        updateButtonAppearance()
        updateStatusLabel(isSystemActive and "Sistema Ativo" or "Sistema Pausado")
    end

    task.wait(0.5)
    if toggleButton then
        toggleButton.MouseButton1Click:Connect(toggleSystem)
        toggleButton.Activated:Connect(toggleSystem)
    end
    local inputConnection = UserInputService.InputBegan:Connect(function(input, gameProcessed)
        if gameProcessed then return end
        if input.KeyCode == Enum.KeyCode.T then toggleSystem() end
    end)
    table.insert(self.Connections, inputConnection)

    player.CharacterAdded:Connect(function(newChar)
        character = newChar
        humanoidRootPart = character:WaitForChild("HumanoidRootPart")
        currentTargetIndex = 0
        allEnemiesDead = false
    end)

    local function getOrCreateHighlight(object)
        if highlightCache[object] then return highlightCache[object] end
        local highlight = object:FindFirstChild("Highlight")
        if not highlight then
            highlight = Instance.new("Highlight")
            highlight.Name = "Highlight"
            highlight.FillColor = CONFIG_TP.HIGHLIGHT_COLOR
            highlight.OutlineColor = CONFIG_TP.OUTLINE_COLOR
            highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
            highlight.FillTransparency = 0.5
            highlight.OutlineTransparency = 0
            highlight.Parent = object
        end
        highlightCache[object] = highlight
        return highlight
    end

    local function cleanupHighlights()
        for object, highlight in pairs(highlightCache) do
            if not object:IsDescendantOf(workspace) or not object.Parent then
                if highlight and highlight.Parent then highlight:Destroy() end
                highlightCache[object] = nil
            end
        end
    end

    local function updatePartsCache()
        local validParts = {}
        for _, part in ipairs(serverFolder:GetChildren()) do
            if part:IsA("BasePart") or part:IsA("MeshPart") then
                table.insert(validParts, part)
            end
        end
        partsCache = validParts
        if currentTargetIndex > #validParts then currentTargetIndex = 0 end
        return validParts
    end

    local function getNextPart()
        local parts = partsCache
        if #parts == 0 then return nil end
        currentTargetIndex = currentTargetIndex + 1
        if currentTargetIndex > #parts then currentTargetIndex = 1 end
        return parts[currentTargetIndex]
    end

    local function checkAllEnemiesDead()
        local parts = updatePartsCache()
        return #parts == 0
    end

    local function findFirePortal()
        for _, model in ipairs(worldFolder:GetChildren()) do
            if model:IsA("Model") then
                local firePortal = model:FindFirstChild("FirePortal", true)
                if firePortal then return firePortal end
            end
        end
        return nil
    end

    local function activateProximityPrompt(portal)
        if not portal then return false end
        local proximityPrompt = portal:FindFirstChildOfClass("ProximityPrompt", true)
        if not proximityPrompt then
            for _, child in ipairs(portal:GetDescendants()) do
                if child:IsA("ProximityPrompt") then
                    proximityPrompt = child
                    break
                end
            end
        end
        if proximityPrompt then
            if portal:IsA("BasePart") or portal:IsA("MeshPart") then
                humanoidRootPart.CFrame = CFrame.new(portal.Position)
            elseif portal:IsA("Model") and portal.PrimaryPart then
                humanoidRootPart.CFrame = CFrame.new(portal.PrimaryPart.Position)
            end
            task.wait(0.1)
            proximityPrompt:InputHoldBegin()
            task.wait(CONFIG_TP.PROXIMITY_HOLD_TIME)
            proximityPrompt:InputHoldEnd()
            updateStatusLabel("Portal ativado!")
            return true
        end
        return false
    end

    local function teleportToPortal()
        local portal = findFirePortal()
        if portal then
            updateStatusLabel("Indo ao portal...")
            local targetPos
            if portal:IsA("BasePart") or portal:IsA("MeshPart") then
                targetPos = portal.Position
            elseif portal:IsA("Model") and portal.PrimaryPart then
                targetPos = portal.PrimaryPart.Position
            else
                local firstPart = portal:FindFirstChildWhichIsA("BasePart", true)
                if firstPart then targetPos = firstPart.Position end
            end
            if targetPos then
                humanoidRootPart.CFrame = CFrame.new(targetPos)
                task.wait(0.2)
                activateProximityPrompt(portal)
                return true
            end
        end
        return false
    end

    local function teleportToPart(part)
        if not part or not character or not humanoidRootPart then return false end
        if not part.Parent then return false end
        local offset = Vector3.new(CONFIG_TP.TELEPORT_DISTANCE, 0, 0)
        local targetCFrame = CFrame.new(part.Position + offset + CONFIG_TP.TELEPORT_OFFSET)
        humanoidRootPart.CFrame = targetCFrame
        updateStatusLabel(string.format("Inimigo %d/%d", currentTargetIndex, #partsCache))
        return true
    end

    local function autoTeleportLoop()
        while task.wait(0.5) do
            if not isSystemActive then
                updateStatusLabel("Sistema Pausado")
                continue
            end
            if isTeleporting then continue end
            if checkAllEnemiesDead() and not allEnemiesDead then
                allEnemiesDead = true
                updateStatusLabel("Procurando portal...")
                task.wait(0.5)
                if not teleportToPortal() then
                    updateStatusLabel("Aguardando portal...")
                end
            elseif not checkAllEnemiesDead() then
                allEnemiesDead = false
                local nextPart = getNextPart()
                if nextPart then
                    isTeleporting = true
                    teleportToPart(nextPart)
                    task.wait(0.1)
                    isTeleporting = false
                end
            end
        end
    end

    local function applyHighlights()
        for _, part in ipairs(serverFolder:GetChildren()) do
            if (part:IsA("BasePart") or part:IsA("MeshPart")) and part:IsDescendantOf(workspace) then
                pcall(function() getOrCreateHighlight(part) end)
            end
        end
    end

    local portalConnection = worldFolder.DescendantAdded:Connect(function(descendant)
        if descendant.Name == "FirePortal" then
            if allEnemiesDead and isSystemActive then
                task.wait(0.5)
                teleportToPortal()
            end
        end
    end)
    table.insert(self.Connections, portalConnection)

    local enemyAddedConnection = serverFolder.ChildAdded:Connect(function(child)
        task.wait(0.05)
        if child:IsA("BasePart") or child:IsA("MeshPart") then
            updatePartsCache()
            allEnemiesDead = false
            updateStatusLabel(string.format("%d inimigos", #partsCache))
        end
    end)
    table.insert(self.Connections, enemyAddedConnection)

    local enemyRemovedConnection = serverFolder.ChildRemoved:Connect(function(child)
        updatePartsCache()
        updateStatusLabel(string.format("%d inimigos", #partsCache))
    end)
    table.insert(self.Connections, enemyRemovedConnection)

    local heartbeatConnection = RunService.Heartbeat:Connect(function()
        if not isSystemActive then return end
        local now = tick()
        if now - lastUpdate < CONFIG_TP.UPDATE_INTERVAL then return end
        lastUpdate = now
        cleanupHighlights()
        updatePartsCache()
        applyHighlights()
    end)
    table.insert(self.Connections, heartbeatConnection)

    updatePartsCache()
    local totalParts = #partsCache
    if totalParts > 0 then
        updateStatusLabel(string.format("%d inimigos", totalParts))
    else
        updateStatusLabel("Aguardando...")
    end

    task.spawn(autoTeleportLoop)
    Fluent:Notify({Title = "Auto Dungeon/INF", Content = "✅ Sistema ativado!", Duration = 4})
end

--==================== LOOP AUTO (SHOP, UPGRADE, GACHAS) ====================
-- Auto-stop gacha por equip (procura equip final, evita gastar mais)
local StopGacha = {}
for i, gName in ipairs(GachaRemoteNames) do
    StopGacha[gName] = false
end

Remote.OnClientEvent:Connect(function(args)
    local ok, data = pcall(function() return args[1][1] end)
    if not ok or not data then return end
    if data.Equip and data.GachaName then
        for i, gName in ipairs(GachaRemoteNames) do
            if data.GachaName == gName then
                -- se equip termina com "8" marca como stop (exemplo baseado no outro script)
                if string.sub(tostring(data.Equip), -1) == "8" then
                    StopGacha[gName] = true
                    print("Raridade máxima encontrada em:", gName, "Equip:", data.Equip)
                end
            end
        end
    end
end)

task.spawn(function()
    while true do
        task.wait(0.35)
        if Toggles.Exp then ComprarItem("RuneShop", "DgExpRune") end
        if Toggles.Core then ComprarItem("RuneShop", "DgCoreRune") end
        if Toggles.Token then ComprarItem("RuneShop", "DgTokenRune") end
        if Toggles.MoreRoom then ComprarItem("RuneShop", "DgMoreRoomRune") end
        if Toggles.DungeonRank then ComprarItem("ExchangeShop", "DgURankUpRune") end
        if Toggles.DoubleDungeon then ComprarItem("ExchangeShop", "DgDoubleDungeonRune") end
        if Toggles.AutoUpgradeGacha1 then FazerUpgrade("Gacha1","Gacha1") end
        if Toggles.AutoUpgradeGacha2 then FazerUpgrade("Gacha2","Gacha2") end
        if Toggles.AutoUpgradeRunes then FazerUpgrade("Runes","Runes") end
        if Toggles.AutoUpgradeResource then FazerUpgrade("Resource","Resource") end
        if Toggles.AutoUpgradeKey then FazerUpgrade("Keys","Keys") end
        if Toggles.AutoUpgradeBoosts then FazerUpgrade("Boosts","Boosts") end
        if Toggles.AutoUpgradeCores then FazerUpgrade("Cores","Cores") end
        if Toggles.AutoUpgradeTokens then FazerUpgrade("Tokens","Tokens") end

        -- Upgrade Gachas: Shadow e Weapons
        for i, name in ipairs(ShadowGUI) do
            if Toggles[name.."Shadow"] then
                local rem = ShadowRemote[name]
                if rem then FazerUpgrade("Shadow", rem) end
            end
        end
        for i, name in ipairs(WeaponsGUI) do
            if Toggles[name.."Weapons"] then
                local rem = WeaponsRemote[name]
                if rem then FazerUpgrade("Weapons", rem) end
            end
        end

        -- Gachas com auto-stop
        for i, guiName in ipairs(GachaGUI) do
            local gachaName = GachaRemoteNames[i]
            if Toggles[guiName] and not StopGacha[gachaName] then
                FazerGacha(gachaName)
            end
        end
    end
end)

--==================== FLUENT UI CRIAÇÃO =====================
local Window = Fluent:CreateWindow{
    Title = "FOUR HUB", SubTitle = "v3.0 - COMPLETO", TabWidth = 160, Size = UDim2.fromOffset(600, 500),
    Acrylic = true, Theme = "Dark", MinimizeKey = Enum.KeyCode.RightShift
}

local LojaTab = Window:AddTab({Title = "Loja", Icon = "shopping-cart"})
local GachaTab = Window:AddTab({Title = "Gachas", Icon = "gift"})
local UpgradeTab = Window:AddTab({Title = "Upgrades", Icon = "arrow-up"})
local DungeonTab = Window:AddTab({Title = "Dungeon", Icon = "swords"})
local TeleportTab = Window:AddTab({Title = "Teleport", Icon = "map"})
local InfiniteTab = Window:AddTab({ Title = "Infinite", Icon = "infinity" })
local MiscTab = Window:AddTab({Title = "Misc", Icon = "settings"})

-- Castelo 🏰
local TabCasteloSection = InfiniteTab:AddSection("Castelo Infinito🏰")
local dropdownCastelo = TabCasteloSection:AddDropdown("CasteloDropdown", {
    Title = "Selecione o Checkpoint",
    Values = checkpointsCastelo,
    Multi = false
})
TabCasteloSection:AddButton({
    Title = "Criar sala + Iniciar 🏰",
    Callback = function()
        local value = dropdownCastelo.Value
        if value then
            CreateRoom("InfiniteCastleAction")
            task.wait(0.3)
            StartRun("InfiniteCastleAction", value)
            Fluent:Notify({Title = "Infinite", Content = "✅ Castelo: sala criada e iniciada ("..tostring(value).."s)", Duration = 3})
        else
            Fluent:Notify({Title = "Infinite", Content = "❌ Selecione um checkpoint!", Duration = 2})
        end
    end
})

-- Deserto 🏜️
local TabDesertoSection = InfiniteTab:AddSection("Deserto 🏜️")
local dropdownDeserto = TabDesertoSection:AddDropdown("DesertoDropdown", {
    Title = "Selecione o Checkpoint",
    Values = checkpointsDeserto,
    Multi = false
})
TabDesertoSection:AddButton({
    Title = "Criar sala + Iniciar 🏜️",
    Callback = function()
        local value = dropdownDeserto.Value
        if value then
            CreateRoom("InfiniteModeAction")
            task.wait(0.3)
            StartRun("InfiniteModeAction", value)
            Fluent:Notify({Title = "Infinite", Content = "✅ Deserto: sala criada e iniciada ("..tostring(value).."s)", Duration = 3})
        else
            Fluent:Notify({Title = "Infinite", Content = "❌ Selecione um checkpoint!", Duration = 2})
        end
    end
})

-- BossRush 💀
local TabBossRushSection = InfiniteTab:AddSection("BossRush 💀")
TabBossRushSection:AddButton({Title = "Criar Boss 💀", Callback = function()
    CreateBoss()
    Fluent:Notify({Title = "BossRush", Content = "✅ Sala de boss criada!", Duration = 3})
end})
for _, diff in ipairs(bossList) do
    TabBossRushSection:AddButton({
        Title = diff .. " 💥",
        Callback = function()
            CreateBoss()
            task.wait(0.3)
            StartBoss(diff)
            Fluent:Notify({Title = "BossRush", Content = "✅ Boss iniciado: "..diff, Duration = 3})
        end
    })
end

--==================== (continuação restante da UI original) =====================

--==== LOJA ====--
local RuneGroup = LojaTab:AddSection("Rune Shop")
RuneGroup:AddToggle("Exp", {Title = "Auto Exp Rune", Default = false, Callback = function(v) Toggles.Exp = v end})
RuneGroup:AddToggle("Core", {Title = "Auto Core Rune", Default = false, Callback = function(v) Toggles.Core = v end})
RuneGroup:AddToggle("Token", {Title = "Auto Token Rune", Default = false, Callback = function(v) Toggles.Token = v end})
RuneGroup:AddToggle("MoreRoom", {Title = "Auto More Room", Default = false, Callback = function(v) Toggles.MoreRoom = v end})

local ExchangeGroup = LojaTab:AddSection("Exchange Shop")
ExchangeGroup:AddToggle("DungeonRank", {Title = "Auto Dungeon Rankup", Default = false, Callback = function(v) Toggles.DungeonRank = v end})
ExchangeGroup:AddToggle("DoubleDungeon", {Title = "Auto Double Dungeon", Default = false, Callback = function(v) Toggles.DoubleDungeon = v end})

--==== GACHAS ====--
local GachaGroup = GachaTab:AddSection("Auto Gachas")
for i = 1, 8 do
    local guiName = GachaGUI[i]
    GachaGroup:AddToggle(guiName, {Title = guiName, Default = false, Callback = function(v) Toggles[guiName] = v end})
end

local GachaGroup2 = GachaTab:AddSection("Auto Gachas (cont.)")
for i = 9, 16 do
    local guiName = GachaGUI[i]
    GachaGroup2:AddToggle(guiName, {Title = guiName, Default = false, Callback = function(v) Toggles[guiName] = v end})
end

--==== UPGRADES ====--
local UpgradeGachaGroup = UpgradeTab:AddSection("Upgrade Gachas (básico)")
UpgradeGachaGroup:AddToggle("AutoUpgradeGacha1", {Title = "Gacha 1", Default = false, Callback = function(v) Toggles.AutoUpgradeGacha1 = v end})
UpgradeGachaGroup:AddToggle("AutoUpgradeGacha2", {Title = "Gacha 2", Default = false, Callback = function(v) Toggles.AutoUpgradeGacha2 = v end})

local UpgradeGroup = UpgradeTab:AddSection("Upgrade Bag")
UpgradeGroup:AddToggle("AutoUpgradeRunes", {Title = "Runes Bag", Default = false, Callback = function(v) Toggles.AutoUpgradeRunes = v end})
UpgradeGroup:AddToggle("AutoUpgradeResource", {Title = "Resource Bag", Default = false, Callback = function(v) Toggles.AutoUpgradeResource = v end})
UpgradeGroup:AddToggle("AutoUpgradeKey", {Title = "Key Bag", Default = false, Callback = function(v) Toggles.AutoUpgradeKey = v end})
UpgradeGroup:AddToggle("AutoUpgradeBoosts", {Title = "Boosts Bag", Default = false, Callback = function(v) Toggles.AutoUpgradeBoosts = v end})
UpgradeGroup:AddToggle("AutoUpgradeCores", {Title = "Cores Bag", Default = false, Callback = function(v) Toggles.AutoUpgradeCores = v end})
UpgradeGroup:AddToggle("AutoUpgradeTokens", {Title = "Tokens Bag", Default = false, Callback = function(v) Toggles.AutoUpgradeTokens = v end})

--==== UPGRADE GACHA (Shadow / Weapons) UI ====
local UpgradeGachaTab = UpgradeTab:AddSection("Upgrade Gachas (Shadow)")
for i, name in ipairs(ShadowGUI) do
    UpgradeGachaTab:AddToggle(name.."Shadow", {Title = name, Default = false, Callback = function(v) Toggles[name.."Shadow"] = v end})
end

local UpgradeWeaponsTab = UpgradeTab:AddSection("Upgrade Gachas (Weapons)")
for i, name in ipairs(WeaponsGUI) do
    UpgradeWeaponsTab:AddToggle(name.."Weapons", {Title = name, Default = false, Callback = function(v) Toggles[name.."Weapons"] = v end})
end

--==== DUNGEON ====--
local DungeonGroup = DungeonTab:AddSection("Dungeon Options")
DungeonGroup:AddButton({Title = "Criar Dungeon", Callback = function()
    CreateDungeon()
    Fluent:Notify({Title = "Dungeon", Content = "✅ Criada!", Duration = 3})
end})
DungeonGroup:AddButton({Title = "Iniciar Dungeon", Callback = function()
    StartDungeon()
    Fluent:Notify({Title = "Dungeon", Content = "✅ Iniciada!", Duration = 3})
end})

local AutoDungeonGroup = DungeonTab:AddSection("Auto Dungeon")
AutoDungeonGroup:AddToggle("AutoDungeonSmart", {Title = "Auto Dungeon Inteligente", Default = false, Callback = function(v)
    Toggles.AutoDungeonSmart = v
    if v then AutoDungeonSmart:Start() else AutoDungeonSmart:Stop() end
end})
AutoDungeonGroup:AddToggle("AutoDungeonINF", {Title = "Auto Dungeon/INF", Default = false, Callback = function(v)
    Toggles.AutoDungeonINF = v
    if v then TeleportSystem:Initialize() else TeleportSystem:Stop() end
end})

local AutoFarmGroup = DungeonTab:AddSection("Auto Farm")
AutoFarmGroup:AddToggle("AutoFarmCompleto", {Title = "Auto Farm Completo", Default = false, Callback = function(v)
    Toggles.AutoFarmCompleto = v
    if v then AutoFarmSystem:Start() else AutoFarmSystem:Stop() end
end})

--==== TELEPORT ====--
local WorldFolder = workspace:WaitForChild("__Main"):WaitForChild("__World")
local SpawnsFolderTP = workspace:WaitForChild("__Extra"):WaitForChild("__Spawns")

local function TeleportToIsland(islandName)
    local spawnPoint = SpawnsFolderTP:FindFirstChild(islandName)
    if spawnPoint then
        HumanoidRootPart.CFrame = spawnPoint.CFrame + Vector3.new(0, 3, 0)
        Fluent:Notify({Title = "Teleporte", Content = "✅ " .. islandName, Duration = 3})
        return true
    end
    local island = WorldFolder:FindFirstChild(islandName)
    if island and island:IsA("Model") and island.PrimaryPart then
        HumanoidRootPart.CFrame = island.PrimaryPart.CFrame + Vector3.new(0, 5, 0)
        Fluent:Notify({Title = "Teleporte", Content = "✅ " .. islandName, Duration = 3})
        return true
    end
    Fluent:Notify({Title = "Teleporte", Content = "❌ " .. islandName, Duration = 3})
    return false
end

local function GetAvailableIslands()
    local islands = {}
    for _, spawn in ipairs(SpawnsFolderTP:GetChildren()) do
        if spawn:IsA("BasePart") or spawn:IsA("Model") then
            table.insert(islands, spawn.Name)
        end
    end
    local uniqueIslands = {}
    local seen = {}
    for _, island in ipairs(islands) do
        if not seen[island] then
            table.insert(uniqueIslands, island)
            seen[island] = true
        end
    end
    table.sort(uniqueIslands)
    return uniqueIslands
end

local TeleportGroup = TeleportTab:AddSection("Ilhas")
local availableIslands = GetAvailableIslands()
if #availableIslands > 0 then
    for _, islandName in ipairs(availableIslands) do
        TeleportGroup:AddButton({Title = "🌍 " .. islandName, Callback = function()
            TeleportToIsland(islandName)
        end})
    end
end

--==== MISC ====--
local MiscGroup = MiscTab:AddSection("Ferramentas")
MiscGroup:AddButton({Title = "Rejoin", Callback = function()
    game:GetService("TeleportService"):Teleport(game.PlaceId, game.Players.LocalPlayer)
end})

local PerformanceGroup = MiscTab:AddSection("🚀 Performance")
PerformanceGroup:AddButton({Title = "FPS Booster Mobile", Callback = function()
    Toggles.FPSBooster = true
    BoostFPS()
end})

local StatsGroup = MiscTab:AddSection("📊 Stats Dashboard")
StatsGroup:AddToggle("StatsDashboard", {Title = "Ativar Dashboard", Default = false, Callback = function(v)
    Toggles.StatsDashboard = v
    if v then
        CreateStatsDashboard()
        StatsActive = true
        InitialCoins = 0
        InitialExp = 0
        StartTime = tick()
        
        task.spawn(function()
            while StatsActive do
                UpdateStats()
                task.wait(1)
            end
        end)
        
        Fluent:Notify({Title = "Stats Dashboard", Content = "✅ Dashboard ativado!", Duration = 3})
    else
        StatsActive = false
        if StatsUI and StatsUI.ScreenGui then
            StatsUI.ScreenGui:Destroy()
        end
        Fluent:Notify({Title = "Stats Dashboard", Content = "❌ Dashboard desativado!", Duration = 3})
    end
end})

local WebhookGroup = MiscTab:AddSection("🪝 Webhook Logger")

WebhookGroup:AddParagraph({
    Title = "📝 Como Usar",
    Content = "1. Cole sua URL do Discord Webhook\n2. Clique em Salvar e Testar\n3. Use os botões de Log"
})

WebhookGroup:AddInput("WebhookURLInput", {
    Title = "Cole sua URL do Webhook",
    Description = "https://discord.com/api/webhooks/.. .",
    Default = "",
    Placeholder = "https://discord.com/api/webhooks/...",
    Callback = function(Value)
        WEBHOOK_URL = Value or ""
        print("[WEBHOOK] URL Atualizada: " .. tostring(WEBHOOK_URL))
    end
})

WebhookGroup:AddButton({
    Title = "✅ Salvar e Testar",
    Description = "Salva a URL e testa a conexão",
    Callback = function()
        if WEBHOOK_URL == "" or WEBHOOK_URL == nil then
            Fluent:Notify({
                Title = "❌ Webhook",
                Content = "Cole a URL primeiro!",
                Duration = 4
            })
            return
        end
        
        WebhookActive = true
        print("[WEBHOOK] Testando com URL: " ..  WEBHOOK_URL)
        
        SendWebhook(
            "✅ FOUR HUB CONECTADO",
            "Webhook do FOUR HUB v3.0 funcionando!\n\nVocê pode usar os botões para logar eventos.",
            65280
        )
        
        task.wait(1)
        Fluent:Notify({
            Title = "✅ Webhook",
            Content = "URL salva e testada!\nVerifique seu Discord! ",
            Duration = 5
        })
    end
})

WebhookGroup:AddButton({
    Title = "🎁 Log Loot",
    Callback = function()
        if not WebhookActive or WEBHOOK_URL == "" then
            Fluent:Notify({Title = "❌ Erro", Content = "Configure o Webhook primeiro!", Duration = 3})
            return
        end
        LogLoot("Espada Lendária", 1, "Épica")
        Fluent:Notify({Title = "✅ Enviado", Content = "Loot enviado ao Discord!", Duration = 3})
    end
})

WebhookGroup:AddButton({
    Title = "💀 Log Boss",
    Callback = function()
        if not WebhookActive or WEBHOOK_URL == "" then
            Fluent:Notify({Title = "❌ Erro", Content = "Configure o Webhook primeiro!", Duration = 3})
            return
        end
        LogBossKill("Chefe Gigante", "Extremamente Difícil")
        Fluent:Notify({Title = "✅ Enviado", Content = "Boss enviado ao Discord!", Duration = 3})
    end
})

WebhookGroup:AddButton({
    Title = "🏆 Log Achievement",
    Callback = function()
        if not WebhookActive or WEBHOOK_URL == "" then
            Fluent:Notify({Title = "❌ Erro", Content = "Configure o Webhook primeiro!", Duration = 3})
            return
        end
        LogAchievement("Matou 1000 inimigos!")
        Fluent:Notify({Title = "✅ Enviado", Content = "Achievement enviado ao Discord!", Duration = 3})
    end
})

local AntiAFKGroup = MiscTab:AddSection("Proteção")
AntiAFKGroup:AddToggle("AntiAFK", {Title = "Anti AFK", Default = false, Callback = function(v)
    Toggles.AntiAFK = v
    if v then
        local VirtualUser = game:GetService("VirtualUser")
        _G.AntiAFKConnection = game:GetService("Players").LocalPlayer.Idled:Connect(function()
            VirtualUser:CaptureController()
            VirtualUser:ClickButton2(Vector2.new())
        end)
        Fluent:Notify({Title = "Anti AFK", Content = "✅ Ativado!", Duration = 3})
    else
        if _G.AntiAFKConnection then
            _G.AntiAFKConnection:Disconnect()
            _G.AntiAFKConnection = nil
        end
        Fluent:Notify({Title = "Anti AFK", Content = "❌ Desativado!", Duration = 3})
    end
end})

AntiAFKGroup:AddToggle("RemoveTeleportUI", {Title = "Remover TeleportScreen UI", Default = false, Callback = function(v)
    Toggles.RemoveTeleportUI = v
    local ReplicatedStorage = game:GetService("ReplicatedStorage")
    local assetsFolder = ReplicatedStorage:FindFirstChild("__Assets")
    if v then
        local teleportScreen = assetsFolder and assetsFolder:FindFirstChild("TeleportScreen")
        if teleportScreen then
            _G.TeleportScreenOriginalParent = teleportScreen.Parent
            _G.TeleportScreenBackup = teleportScreen
            teleportScreen.Parent = nil
            Fluent:Notify({Title = "TeleportScreen", Content = "✅ Removida!", Duration = 3})
        end
    else
        if _G.TeleportScreenBackup and _G.TeleportScreenOriginalParent then
            _G.TeleportScreenBackup.Parent = _G.TeleportScreenOriginalParent
            Fluent:Notify({Title = "TeleportScreen", Content = "✅ Restaurada!", Duration = 3})
        end
    end
end})

AntiAFKGroup:AddToggle("AutoExecute", {Title = "Auto Execute", Default = false, Callback = function(v)
    Toggles.AutoExecute = v
    if v then
        if not isfolder("FourHub") then pcall(function() makefolder("FourHub") end) end
        pcall(function() writefile("FourHub/AutoExecute.txt", "enabled") end)
        Fluent:Notify({Title = "Auto Execute", Content = "✅ Salvo!", Duration = 3})
    else
        if isfile and isfile("FourHub/AutoExecute.txt") then pcall(function() delfile("FourHub/AutoExecute.txt") end) end
        Fluent:Notify({Title = "Auto Execute", Content = "❌ Desativado!", Duration = 3})
    end
end})

-- Seleciona aba inicial
Window:SelectTab(LojaTab)

-- Verifica Auto Execute
if isfile and isfile("FourHub/AutoExecute.txt") then
    Fluent:Notify({Title = "FOUR HUB v3.0", Content = "✅ Auto Execute detectado!\n🚀 Hub carregado com sucesso!", Duration = 5})
else
    Fluent:Notify({Title = "FOUR HUB v3.0", Content = "✅ Hub carregado!\n📖 Use: Loja | Gachas | Dungeon | Webhook | Infinite ♾️", Duration = 4})
end

print("[FOUR HUB] ✅ Script carregado com sucesso!")
print("[FOUR HUB] 🪝 Configure seu Webhook em Misc > Webhook Logger")
