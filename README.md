-- CIRILOMI HUB - ESP PALHAÇO + SERVER HOP
repeat task.wait() until game:IsLoaded()

if game.CoreGui:FindFirstChild("CiriloClownESP") then game.CoreGui.CiriloClownESP:Destroy() end

local gui = Instance.new("ScreenGui", gethui and gethui() or game.CoreGui)
gui.Name = "CiriloClownESP"
gui.ResetOnSpawn = false

local main = Instance.new("Frame", gui)
main.Size = UDim2.fromOffset(260, 340)
main.Position = UDim2.fromScale(0.05, 0.3)
main.BackgroundColor3 = Color3.fromRGB(15,15,15)
main.BorderSizePixel = 0
Instance.new("UICorner", main).CornerRadius = UDim.new(0,8)
main.Active = true
main.Draggable = true

local title = Instance.new("TextLabel", main)
title.Size = UDim2.fromOffset(260,35)
title.Text = " CiriloMi - ESP Palhaço 🤡"
title.TextXAlignment = Enum.TextXAlignment.Left
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 13

local function createBtn(text, y, color, func)
    local b = Instance.new("TextButton", main)
    b.Size = UDim2.fromOffset(230, 30)
    b.Position = UDim2.fromOffset(15, y)
    b.Text = text
    b.BackgroundColor3 = color
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 11
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,6)
    b.MouseButton1Click:Connect(func)
    return b
end

getgenv().ESPClown = false
getgenv().AutoHopGoodClown = false

-- LISTA DOS PALHAÇOS BONS
getgenv().GoodClowns = {
    ["600k"] = true,
    ["1M"] = true
}

local status = Instance.new("TextLabel", main)
status.Size = UDim2.fromOffset(230, 20)
status.Position = UDim2.fromOffset(15, 45)
status.Text = "Status: Procurando..."
status.TextColor3 = Color3.fromRGB(150,150,150)
status.BackgroundTransparency = 1
status.Font = Enum.Font.Gotham
status.TextSize = 11

-- FUNÇÃO ESP
local function addESP(obj, valueText, color, isRainbow)
    if obj:FindFirstChild("CiriloESP") then return end

    local bill = Instance.new("BillboardGui", obj)
    bill.Name = "CiriloESP"
    bill.Size = UDim2.fromOffset(200, 50)
    bill.StudsOffset = Vector3.new(0, 4, 0)
    bill.AlwaysOnTop = true

    local txt = Instance.new("TextLabel", bill)
    txt.Size = UDim2.fromOffset(200, 50)
    txt.BackgroundTransparency = 1
    txt.Text = "🤡 PALHAÇO\n".. valueText
    txt.TextColor3 = color
    txt.TextStrokeTransparency = 0
    txt.Font = Enum.Font.GothamBold
    txt.TextSize = isRainbow and 16 or 13
    txt.TextStrokeColor3 = Color3.new(0,0,0)

    -- Arco-íris no 1M
    if isRainbow then
        task.spawn(function()
            while bill.Parent do
                for i=0,1,0.01 do
                    if not bill.Parent then break end
                    txt.TextColor3 = Color3.fromHSV(i,1,1)
                    task.wait(0.02)
                end
            end
        end)
        -- Highlight arco-íris
        local hl = Instance.new("Highlight", obj)
        hl.FillTransparency = 0.5
        task.spawn(function()
            while hl.Parent do
                for i=0,1,0.01 do
                    if not hl.Parent then break end
                    hl.FillColor = Color3.fromHSV(i,1,1)
                    task.wait(0.02)
                end
            end
        end)
    else
        local hl = Instance.new("Highlight", obj)
        hl.FillColor = color
        hl.FillTransparency = 0.6
        hl.OutlineColor = Color3.new(1,1,1)
    end
end

local function checkClown(obj)
    local name = obj.Name:lower()
    if not name:find("clown") and not name:find("palha") and not name:find("jester") then return end

    local value = 0
    local text = obj.Name

    -- Pega o valor do nome ou de atributos
    if text:find("100") then value = 100000 text = "100K"
    elseif text:find("150") then value = 150000 text = "150K"
    elseif text:find("200") then value = 200000 text = "200K"
    elseif text:find("300") then value = 300000 text = "300K"
    elseif text:find("600") then value = 600000 text = "600K"
    elseif text:find("1M") or text:find("1000") then value = 1000000 text = "1M - LENDARIO!!!"
    else
        -- tenta achar Value dentro do mob
        local v = obj:FindFirstChild("Value") or obj:FindFirstChild("Bounty") or obj:FindFirstChild("Price")
        if v then value = v.Value end
        text = tostring(value)
    end

    local color = Color3.new(1,1,1)
    local isRainbow = false

    if value >= 1000000 then color = Color3.fromRGB(255,0,0) isRainbow = true
    elseif value >= 600000 then color = Color3.fromRGB(255, 165, 0) -- laranja
    elseif value >= 300000 then color = Color3.fromRGB(170, 0, 255) -- roxo
    elseif value >= 200000 then color = Color3.fromRGB(0, 170, 255) -- azul
    elseif value >= 150000 then color = Color3.fromRGB(0, 255, 0) -- verde
    elseif value >= 100000 then color = Color3.fromRGB(255,255,255)
    end

    addESP(obj, text, color, isRainbow)

    -- Se for palhaço bom e auto hop ligado
    if getgenv().AutoHopGoodClown and (value >= 600000) then
        status.Text = "ACHOU! ".. text.. " - Ficando no server"
        status.TextColor3 = Color3.fromRGB(0,255,0)
        getgenv().AutoHopGoodClown = false
        game.StarterGui:SetCore("SendNotification", {Title = "CiriloMi ESP", Text = "Palhaço BOM achado: "..text, Duration = 5})
    else
        status.Text = "Palhaço achado: ".. text
    end
end

-- SCAN LOOP
task.spawn(function()
    while true do task.wait(0.5)
        if getgenv().ESPClown then
            pcall(function()
                for _,v in pairs(workspace.Enemies:GetChildren()) do checkClown(v) end
                for _,v in pairs(workspace:GetChildren()) do if v:FindFirstChild("Humanoid") then checkClown(v) end end
                for _,v in pairs(workspace.NPCs:GetChildren()) do checkClown(v) end
            end)
        end
    end
end)

-- SERVER HOP PRA ACHAR PALHAÇO BOM
local function serverHop()
    local Http = game:GetService("HttpService")
    local TPS = game:GetService("TeleportService")
    local servers = Http:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..game.PlaceId.."/servers/Public?sortOrder=Asc&limit=100")).data
    for _,s in pairs(servers) do
        if s.playing < s.maxPlayers and s.id ~= game.JobId then
            TPS:TeleportToPlaceInstance(game.PlaceId, s.id, game.Players.LocalPlayer)
            break
        end
    end
end

createBtn("ESP Palhaço [OFF]", 75, Color3.fromRGB(60,60,60), function()
    getgenv().ESPClown = not getgenv().ESPClown
    status.Text = getgenv().ESPClown and "ESP Ligado - Procurando..." or "ESP Desligado"
end)

createBtn("Trocar Server [Hop]", 115, Color3.fromRGB(120,0,255), function()
    status.Text = "Trocando de servidor..."
    serverHop()
end)

createBtn("Auto Hop até 600K/1M", 155, Color3.fromRGB(0, 170, 0), function()
    getgenv().AutoHopGoodClown = not getgenv().AutoHopGoodClown
    status.Text = getgenv().AutoHopGoodClown and "Auto Hop ON - Só para em 600K/1M" or "Auto Hop OFF"
    task.spawn(function()
        while getgenv().AutoHopGoodClown do
            task.wait(8)
            if getgenv().AutoHopGoodClown then
                -- se não achou nada bom em 8s, troca
                local foundGood = false
                for _,v in pairs(workspace.Enemies:GetChildren()) do
                    if v.Name:lower():find("clown") and (v.Name:find("600") or v.Name:find("1M")) then foundGood = true end
                end
                if not foundGood then serverHop() end
            end
        end
    end)
end)

createBtn("Limpar ESP", 195, Color3.fromRGB(80,80,80), function()
    for _,v in pairs(workspace:GetDescendants()) do
        if v.Name == "CiriloESP" or v:IsA("Highlight") then v:Destroy() end
    end
    status.Text = "ESP Limpo"
end)

-- LEGENDA
local legend = Instance.new("TextLabel", main)
legend.Size = UDim2.fromOffset(230, 90)
legend.Position = UDim2.fromOffset(15, 235)
legend.Text = "🤡 LEGENDA:\n100K=Branco 150K=Verde\n200K=Azul 300K=Roxo\n600K=Laranja 1M=ARCO-ÍRIS"
legend.TextXAlignment = Left
legend.BackgroundTransparency = 1
legend.TextColor3 = Color3.fromRGB(200,200,200)
legend.Font = Enum.Font.Gotham
legend.TextSize = 11
legend.TextYAlignment = Top

print("CiriloMi ESP Palhaço carregado!")
