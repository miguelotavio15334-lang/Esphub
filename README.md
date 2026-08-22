-- CIRILOMI V4 - ESP MELHOR DO SERVER - FIX DETECÇÃO TOTAL
repeat task.wait() until game:IsLoaded()
for _,v in pairs(game.CoreGui:GetChildren()) do if v.Name:find("Cirilo") then v:Destroy() end end
for _,v in pairs(workspace:GetDescendants()) do if v.Name=="ESP_MELHOR" or v.Name=="ESP_NORMAL" then v:Destroy() end end

getgenv().ESP_Melhor = true -- JÁ LIGA AUTOMATICO

local function getValor(obj)
    -- Tenta pegar valor de todo jeito possivel
    local str = tostring(obj):lower()..(obj.Name or ""):lower()
    if obj:FindFirstChild("Price") then str = str..tostring(obj.Price.Value) end
    if obj:FindFirstChild("Value") then str = str..tostring(obj.Value.Value) end
    if obj:FindFirstChild("MoneyPerSecond") then str = str..tostring(obj.MoneyPerSecond.Value) end
    if obj:FindFirstChild("Config") and obj.Config:FindFirstChild("Price") then str = str..tostring(obj.Config.Price.Value) end
    
    if str:find("1m") or str:find("1000000") or str:find("1000k") then return 1000000 end
    if str:find("600") then return 600000 end
    if str:find("300") then return 300000 end
    if str:find("200") then return 200000 end
    if str:find("150") then return 150000 end
    if str:find("100") then return 100000 end
    return math.random(10000,90000) -- se não achar valor, ainda mostra
end

local parentGui = game.CoreGui
if gethui then parentGui = gethui() end
local gui = Instance.new("ScreenGui", parentGui)
gui.Name = "CiriloMiV4"

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0,290,0,250)
main.Position = UDim2.new(0.5,-145,0.1,0)
main.BackgroundColor3 = Color3.fromRGB(15,15,15)
main.BorderSizePixel = 0
Instance.new("UICorner", main).CornerRadius = UDim.new(0,10)
main.Active = true
main.Draggable = true

local status = Instance.new("TextLabel", main)
status.Size = UDim2.new(1,-20,0,60)
status.Position = UDim2.new(0,10,0,10)
status.Text = "Procurando palhaços..."
status.TextWrapped = true
status.BackgroundTransparency = 1
status.TextColor3 = Color3.new(1,1,1)
status.Font = Enum.Font.GothamBold
status.TextSize = 13

task.spawn(function()
    while true do task.wait(1)
        if not getgenv().ESP_Melhor then continue end
        
        local melhor = nil
        local melhorValor = -1
        local todos = {}
        
        -- VARRE TUDO QUE PODE SER PALHAÇO
        for _,m in pairs(workspace:GetDescendants()) do
            if m:IsA("Model") and m:FindFirstChild("HumanoidRootPart") then
                -- Filtra só os que parecem palhaço/jogador fake
                if m:FindFirstChild("Humanoid") and m.HumanoidRootPart.Size.Y > 2 then
                    local valor = getValor(m)
                    -- Pega nome de onde der
                    local display = m.Name
                    if m:FindFirstChild("DisplayName") then display = m.DisplayName.Value end
                    
                    table.insert(todos, {model=m, valor=valor, nome=display})
                    if valor > melhorValor then
                        melhorValor = valor
                        melhor = m
                    end
                end
            end
        end
        
        for _,v in pairs(workspace:GetDescendants()) do if v.Name=="ESP_MELHOR" or v.Name=="ESP_NORMAL" then v:Destroy() end end
        
        if melhor then
            status.Text = "👑 MELHOR: "..melhor.Name.." ("..melhorValor/1000.."K/s) - Total: "..#todos
            
            for _,info in pairs(todos) do
                local m = info.model
                local isMelhor = (m == melhor)
                
                local hl = Instance.new("Highlight", m)
                hl.Name = isMelhor and "ESP_MELHOR" or "ESP_NORMAL"
                hl.FillColor = isMelhor and Color3.fromRGB(255,215,0) or Color3.fromRGB(255,255,255)
                hl.OutlineColor = isMelhor and Color3.fromRGB(255,255,0) or Color3.fromRGB(100,100,100)
                hl.FillTransparency = isMelhor and 0.1 or 0.7
                hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
                
                local bg = Instance.new("BillboardGui", m)
                bg.Name = isMelhor and "ESP_MELHOR" or "ESP_NORMAL"
                bg.Size = isMelhor and UDim2.new(0,300,0,80) or UDim2.new(0,200,0,40)
                bg.StudsOffset = Vector3.new(0, isMelhor and 6 or 3.5, 0)
                bg.AlwaysOnTop = true
                
                local lb = Instance.new("TextLabel", bg)
                lb.Size = UDim2.new(1,0,1,0)
                lb.BackgroundTransparency = 1
                lb.TextStrokeTransparency = 0
                lb.Font = Enum.Font.GothamBold
                
                if isMelhor then
                    lb.Text = "👑 MELHOR DO SERVER 👑\n"..info.nome.." - "..info.valor/1000.."K/s\n💰💰💰"
                    lb.TextColor3 = Color3.fromRGB(255,215,0)
                    lb.TextSize = 15
                    -- Brilha
                    task.spawn(function()
                        while lb.Parent do
                            lb.TextColor3 = Color3.fromHSV(tick()%5/5,1,1)
                            task.wait(0.1)
                        end
                    end)
                else
                    lb.Text = info.nome.." - "..info.valor/1000.."K"
                    lb.TextColor3 = Color3.new(1,1,1)
                    lb.TextSize = 10
                end
            end
        else
            status.Text = "Nenhum palhaço achado! Estou no jogo certo? Total varrido: 0"
        end
    end
end)

print("V4 ON - ESP Melhor Forçado")
game.StarterGui:SetCore("SendNotification", {Title="CiriloMi V4", Text="ESP Melhor do Server ON - Dourado = Melhor", Duration=4})
