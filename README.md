-- CIRILOMI V3 - ESP MELHOR DO SERVER (MONEY)
repeat task.wait() until game:IsLoaded()
for _,v in pairs(game.CoreGui:GetChildren()) do if v.Name:find("Cirilo") then v:Destroy() end end

getgenv().ESP_Melhor = false
getgenv().Hop = false

local function getValor(nome)
    local n = nome:lower()
    if n:find("1m") or n:find("1000") then return 1000000 end
    if n:find("600") then return 600000 end
    if n:find("300") then return 300000 end
    if n:find("200") then return 200000 end
    if n:find("150") then return 150000 end
    if n:find("100") then return 100000 end
    return 0
end

local function getCor(v)
    if v >= 1000000 then return Color3.fromRGB(255,0,255) end
    if v >= 600000 then return Color3.fromRGB(255,140,0) end
    if v >= 300000 then return Color3.fromRGB(130,0,255) end
    if v >= 200000 then return Color3.fromRGB(0,150,255) end
    if v >= 150000 then return Color3.fromRGB(0,255,0) end
    return Color3.new(1,1,1)
end

-- GUI NOVA
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "CiriloMiV3"
local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0,290,0,420)
main.Position = UDim2.new(0.5,-145,0.5,-210)
main.BackgroundColor3 = Color3.fromRGB(12,12,12)
main.BorderSizePixel = 0
Instance.new("UICorner", main).CornerRadius = UDim.new(0,12)
main.Active = true
main.Draggable = true
local stroke = Instance.new("UIStroke", main)
stroke.Color = Color3.fromRGB(130,0,255)
stroke.Thickness = 2

local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1,0,0,35)
title.Text = " CiriloMi - ESP Palhaço 🤡"
title.Font = Enum.Font.GothamBold
title.TextSize = 14
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundTransparency = 1
title.TextXAlignment = Enum.TextXAlignment.Left

local status = Instance.new("TextLabel", main)
status.Size = UDim2.new(1,0,0,18)
status.Position = UDim2.new(0,0,0,32)
status.Text = "ESP Ligado - Procurando..."
status.Font = Enum.Font.Gotham
status.TextSize = 11
status.TextColor3 = Color3.fromRGB(180,180,180)
status.BackgroundTransparency = 1

local function btn(txt, y, cor, func)
    local b = Instance.new("TextButton", main)
    b.Size = UDim2.new(1,-20,0,42)
    b.Position = UDim2.new(0,10,0,y)
    b.Text = txt
    b.BackgroundColor3 = cor
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 12
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,8)
    b.MouseButton1Click:Connect(function() func(b) end)
    return b
end

-- LOOP DO ESP MELHOR
task.spawn(function()
    while true do task.wait(0.5)
        if not getgenv().ESP_Melhor then
            -- limpa se tiver desligado
            for _,v in pairs(workspace:GetDescendants()) do if v.Name == "ESP_MELHOR" or v.Name == "ESP_NORMAL" then v:Destroy() end end
            task.wait(1)
            continue
        end

        local melhor = nil
        local melhorValor = 0
        local todos = {}

        -- ACHA TODOS
        for _,m in pairs(workspace:GetDescendants()) do
            if m:IsA("Model") and m:FindFirstChild("HumanoidRootPart") and m:FindFirstChild("Humanoid") then
                if m.Name:lower():find("clown") or m.Name:lower():find("jester") or m.Name:lower():find("palha") then
                    local v = getValor(m.Name)
                    table.insert(todos, {model=m, valor=v})
                    if v > melhorValor then
                        melhorValor = v
                        melhor = m
                    end
                end
            end
        end

        -- LIMPA ESP ANTIGO
        for _,v in pairs(workspace:GetDescendants()) do if v.Name == "ESP_MELHOR" or v.Name == "ESP_NORMAL" then v:Destroy() end end

        if melhor then
            status.Text = "MELHOR DO SERVER: "..melhor.Name.." - "..melhorValor/1000.."K"

            for _,info in pairs(todos) do
                local m = info.model
                local v = info.valor
                local isMelhor = (m == melhor)

                -- Highlight
                local hl = Instance.new("Highlight", m)
                hl.Name = isMelhor and "ESP_MELHOR" or "ESP_NORMAL"
                hl.FillColor = isMelhor and Color3.fromRGB(255,215,0) or getCor(v)
                hl.OutlineColor = isMelhor and Color3.new(1,1,0) or Color3.new(1,1,1)
                hl.FillTransparency = isMelhor and 0.2 or 0.6
                hl.OutlineTransparency = 0
                hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop

                -- Billboard
                local bg = Instance.new("BillboardGui", m)
                bg.Name = isMelhor and "ESP_MELHOR" or "ESP_NORMAL"
                bg.Size = isMelhor and UDim2.new(0,250,0,60) or UDim2.new(0,200,0,40)
                bg.StudsOffset = Vector3.new(0, isMelhor and 5.5 or 4, 0)
                bg.AlwaysOnTop = true

                local lb = Instance.new("TextLabel", bg)
                lb.Size = UDim2.new(1,0,1,0)
                lb.BackgroundTransparency = 1
                if isMelhor then
                    lb.Text = "👑 MELHOR DO SERVER 👑\n🤡 "..m.Name.." 💰 "..v/1000.."K/s"
                    lb.TextColor3 = Color3.fromRGB(255,215,0)
                    lb.TextSize = 14
                else
                    lb.Text = "🤡 "..m.Name.." - "..v/1000.."K"
                    lb.TextColor3 = getCor(v)
                    lb.TextSize = 11
                end
                lb.Font = Enum.Font.GothamBold
                lb.TextStrokeTransparency = 0

                if isMelhor and v >= 600000 then
                    task.spawn(function()
                        while lb.Parent do
                            for i=0,1,0.03 do if not lb.Parent then break end lb.TextColor3 = Color3.fromHSV(i,1,1) task.wait() end
                        end
                    end)
                end
            end
        else
            status.Text = "Nenhum palhaço no server - Hopando..."
            if getgenv().Hop then
                game:GetService("TeleportService"):Teleport(game.PlaceId)
            end
        end
    end
end)

-- BOTOES IGUAL DA TUA PRINT + NOVO
btn("ESP Palhaço [OFF]", 60, Color3.fromRGB(60,60,60), function(b)
    getgenv().ESP_Melhor = not getgenv().ESP_Melhor
    b.Text = getgenv().ESP_Melhor and "ESP Palhaço [ON]" or "ESP Palhaço [OFF]"
    b.BackgroundColor3 = getgenv().ESP_Melhor and Color3.fromRGB(0,170,0) or Color3.fromRGB(60,60,60)
end)

btn("Trocar Server [Hop]", 110, Color3.fromRGB(130,0,255), function()
    game:GetService("TeleportService"):Teleport(game.PlaceId)
end)

btn("Auto Hop até 600K/1M", 160, Color3.fromRGB(0,200,0), function(b)
    getgenv().Hop = not getgenv().Hop
    b.Text = getgenv().Hop and "Auto Hop até 600K/1M [ON]" or "Auto Hop até 600K/1M"
    b.BackgroundColor3 = getgenv().Hop and Color3.fromRGB(0,130,0) or Color3.fromRGB(0,200,0)
end)

btn("Limpar ESP", 210, Color3.fromRGB(60,60,60), function()
    getgenv().ESP_Melhor = false
    for _,v in pairs(workspace:GetDescendants()) do if v.Name == "ESP_MELHOR" or v.Name == "ESP_NORMAL" then v:Destroy() end end
end)

local legenda = Instance.new("TextLabel", main)
legenda.Position = UDim2.new(0,10,0,265)
legenda.Size = UDim2.new(1,-20,0,80)
legenda.Text = "🤡 LEGENDA:\n100K=Branco 150K=Verde 200K=Azul 300K=Roxo\n600K=Laranja 1M=ARCO-ÍRIS\n\n👑 DOURADO = O QUE DA MAIS MONEY DO SERVER"
legenda.TextWrapped = true
legenda.BackgroundTransparency = 1
legenda.BackgroundColor3 = Color3.fromRGB(80,80,80)
legenda.TextColor3 = Color3.fromRGB(200,200,200)
legenda.Font = Enum.Font.Gotham
legenda.TextSize = 11
legenda.TextXAlignment = Left
legenda.TextYAlignment = Top
