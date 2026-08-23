-- MIIIGUEX V14 - FOCA NO RAINBOW EPIC DA FOTO
local LP = game.Players.LocalPlayer
pcall(function() LP.PlayerGui:FindFirstChild("MIIIGUEX_HUB"):Destroy() end)

local gui = Instance.new("ScreenGui", LP.PlayerGui)
gui.Name = "MIIIGUEX_HUB"
gui.ResetOnSpawn = false

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 340, 0, 380)
main.Position = UDim2.new(0.5,-170,0.5,-190)
main.BackgroundColor3 = Color3.fromRGB(18,18,22)
main.Active = true
main.Draggable = true
Instance.new("UICorner", main).CornerRadius = UDim.new(0,12)

local function Btn(txt,y,col)
    local b = Instance.new("TextButton", main)
    b.Position = UDim2.new(0.05,0,0,y)
    b.Size = UDim2.new(0.9,0,0,48)
    b.Text = txt
    b.BackgroundColor3 = col or Color3.fromRGB(35,40,55)
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 13
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,10)
    return b
end

local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1,0,0,45)
title.Text = "MIIIGUEX V14 - RAINBOW HUNT"
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 14

local bNo = Btn("NoClip: OFF", 50)
local bAuto = Btn("🤡 ROUBAR RAINBOW/EPIC: OFF", 110, Color3.fromRGB(130,30,30))
local bAll = Btn("💀 ROUBAR TODOS: OFF", 165, Color3.fromRGB(35,40,55))
local bHop = Btn("🔄 AUTO HOP SE VAZIO: OFF", 220, Color3.fromRGB(35,40,55))
local bClose = Btn("Cerrar", 275, Color3.fromRGB(60,60,60))

local noclip, autoRainbow, autoAll, autohop = false,false,false,false

bNo.MouseButton1Click:Connect(function() noclip = not noclip bNo.Text = noclip and "NoClip: ON" or "NoClip: OFF" bNo.BackgroundColor3 = noclip and Color3.fromRGB(0,170,80) or Color3.fromRGB(35,40,55) end)
bAuto.MouseButton1Click:Connect(function() autoRainbow = not autoRainbow bAuto.Text = autoRainbow and "🤡 ROUBAR RAINBOW/EPIC: ON" or "🤡 ROUBAR RAINBOW/EPIC: OFF" bAuto.BackgroundColor3 = autoRainbow and Color3.fromRGB(0,170,80) or Color3.fromRGB(130,30,30) end)
bAll.MouseButton1Click:Connect(function() autoAll = not autoAll bAll.Text = autoAll and "💀 ROUBAR TODOS: ON" or "💀 ROUBAR TODOS: OFF" bAll.BackgroundColor3 = autoAll and Color3.fromRGB(0,170,80) or Color3.fromRGB(35,40,55) end)
bHop.MouseButton1Click:Connect(function() autohop = not autohop bHop.Text = autohop and "🔄 AUTO HOP: ON" or "🔄 AUTO HOP: OFF" bHop.BackgroundColor3 = autohop and Color3.fromRGB(0,170,80) or Color3.fromRGB(35,40,55) end)
bClose.MouseButton1Click:Connect(function() gui:Destroy() end)

game:GetService("RunService").Stepped:Connect(function()
    if noclip or autoRainbow or autoAll then
        pcall(function() for _,v in pairs(LP.Character:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide = false end end end)
    end
end)

local function getMyPlot()
    for _,plot in pairs(workspace:WaitForChild("Plots"):GetChildren()) do
        if plot:FindFirstChild("Owner") and plot.Owner.Value == LP.Name then return plot end
    end
end

local function getScore(model)
    -- calcula nota pro palhaço - igual da tua foto
    local score = 0
    local price = 0
    if model:FindFirstChild("PricePerSecond") then price = model.PricePerSecond.Value end
    
    local text = ""
    for _,v in pairs(model:GetDescendants()) do
        if v:IsA("TextLabel") or v:IsA("BillboardGui") then
            text = text .. " " .. (v.Text or "")
        end
    end
    text = text:lower()

    if text:find("rainbow") then score = score + 10000 end
    if text:find("epic") then score = score + 5000 end
    if text:find("rare") then score = score + 1000 end
    if text:find("dindonsini") then score = score + 2000 end -- o da foto
    if text:find("aaa") then score = score + 500 end

    score = score + price
    return score, price
end

task.spawn(function()
    while true do task.wait(0.18)
        if autoRainbow or autoAll then pcall(function()
            local hrp = LP.Character.HumanoidRootPart
            if LP.Character:FindFirstChildWhichIsA("Tool") then
                local my = getMyPlot()
                if my then hrp.CFrame = my:GetPivot() * CFrame.new(0,6,0) end
            else
                local best = nil
                local bestScore = -1
                for _,plot in pairs(workspace.Plots:GetChildren()) do
                    if plot:FindFirstChild("Owner") and plot.Owner.Value ~= LP.Name then
                        for _,model in pairs(plot:GetChildren()) do
                            if model:IsA("Model") and model.PrimaryPart and model:FindFirstChild("PricePerSecond") then
                                local score, price = getScore(model)
                                -- se for modo RAINBOW, só pega score alto
                                if autoAll then score = price end
                                if autoRainbow and score < 5000 then continue end
                                
                                if score > bestScore then
                                    bestScore = score
                                    best = model
                                end
                            end
                        end
                    end
                end

                if best then
                    hrp.CFrame = best.PrimaryPart.CFrame + Vector3.new(0,0,1.5)
                    task.wait(0.12)
                    firetouchinterest(hrp, best.PrimaryPart, 0)
                    task.wait(0.08)
                    firetouchinterest(hrp, best.PrimaryPart, 1)
                    for _,d in pairs(best:GetDescendants()) do
                        if d:IsA("ProximityPrompt") then
                            d.HoldDuration = 0 d.MaxActivationDistance = 100 fireproximityprompt(d)
                        end
                        if d:IsA("ClickDetector") then fireclickdetector(d) end
                    end
                    -- avisa qual pegou
                    game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX", Text="Tentando roubar: "..best.Name.." Score: "..bestScore, Duration=2})
                else
                    if autohop then
                        game:GetService("TeleportService"):Teleport(game.PlaceId, LP)
                    end
                end
            end
        end) end
    end
end)
