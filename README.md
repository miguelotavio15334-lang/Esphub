-- MIIIGUEX V21 - PEGOU NA MÃO = VOA PRA BASE E KITA
-- Da print STOLEN que tu mandou

local LP = game.Players.LocalPlayer
local RS = game:GetService("RunService")
local TS = game:GetService("TweenService")

pcall(function() LP.PlayerGui:FindFirstChild("MIIIGUEX_V21"):Destroy() end)

local gui = Instance.new("ScreenGui", LP.PlayerGui)
gui.Name = "MIIIGUEX_V21"
gui.ResetOnSpawn = false

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 360, 0, 200)
main.Position = UDim2.new(0.5,-180,0.5,-100)
main.BackgroundColor3 = Color3.fromRGB(24,24,28)
main.Active = true
main.Draggable = true
Instance.new("UICorner", main).CornerRadius = UDim.new(0,16)

local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1,0,0,40)
title.Text = "MIIIGUEX V21 - VOA PRA BASE"
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 14

local function btn(txt,y,col)
    local b = Instance.new("TextButton", main)
    b.Position = UDim2.new(0,15,0,y)
    b.Size = UDim2.new(1,-30,0,48)
    b.Text = txt
    b.BackgroundColor3 = col or Color3.fromRGB(45,47,55)
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 13
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,10)
    return b
end

local bFly = btn("✈️ VOAR PRA BASE AO PEGAR: OFF", 45, Color3.fromRGB(130,30,30))
local bKitar = btn("🔑 AUTO KITAR NA BASE: ON", 100, Color3.fromRGB(0,170,80))
local bClose = btn("Cerrar", 155, Color3.fromRGB(60,60,60))

local flyEnabled = false
local kitarEnabled = true

bFly.MouseButton1Click:Connect(function()
    flyEnabled = not flyEnabled
    bFly.Text = flyEnabled and "✈️ VOAR PRA BASE AO PEGAR: ON" or "✈️ VOAR PRA BASE AO PEGAR: OFF"
    bFly.BackgroundColor3 = flyEnabled and Color3.fromRGB(0,170,80) or Color3.fromRGB(130,30,30)
end)

bKitar.MouseButton1Click:Connect(function()
    kitarEnabled = not kitarEnabled
    bKitar.Text = kitarEnabled and "🔑 AUTO KITAR NA BASE: ON" or "🔑 AUTO KITAR NA BASE: OFF"
    bKitar.BackgroundColor3 = kitarEnabled and Color3.fromRGB(0,170,80) or Color3.fromRGB(130,30,30)
end)

bClose.MouseButton1Click:Connect(function() gui:Destroy() end)

-- NoClip sempre quando voando
RS.Stepped:Connect(function()
    if flyEnabled then
        pcall(function()
            for _,v in pairs(LP.Character:GetDescendants()) do
                if v:IsA("BasePart") then v.CanCollide = false end
            end
        end)
    end
end)

local function getMyPlot()
    for _,p in pairs(workspace:WaitForChild("Plots"):GetChildren()) do
        if p:FindFirstChild("Owner") and p.Owner.Value == LP.Name then
            return p
        end
    end
end

local function voarAte(pos)
    local hrp = LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
    if not hrp then return end
    
    -- voo rapido tipo Tween
    local dist = (hrp.Position - pos).Magnitude
    local speed = 120 -- velocidade do voo
    local time = dist / speed
    if time < 0.2 then time = 0.2 end
    
    local tween = TS:Create(hrp, TweenInfo.new(time, Enum.EasingStyle.Linear), {CFrame = CFrame.new(pos + Vector3.new(0,5,0))})
    tween:Play()
    tween.Completed:Wait()
end

-- DETECTOR: PEGOU NA MÃO
LP.Character.ChildAdded:Connect(function(child)
    if child:IsA("Tool") and flyEnabled then
        task.wait(0.1)
        game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX", Text="Pegou o palhaço! Voando pra base...", Duration=2})
        
        local myPlot = getMyPlot()
        if myPlot then
            -- voa até a base
            voarAte(myPlot:GetPivot().Position)
            task.wait(0.3)
            
            if kitarEnabled then
                -- kita sozinho
                game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX", Text="Kitando na base! 🔑", Duration=2})
                task.wait(0.5)
                local tool = LP.Character:FindFirstChildWhichIsA("Tool")
                if tool then
                    tool.Parent = LP.Backpack
                    -- tenta remote de kitar tambem
                    for _,r in pairs(game.ReplicatedStorage:GetDescendants()) do
                        if r:IsA("RemoteEvent") and r.Name:lower():find("drop") then
                            r:FireServer()
                        end
                    end
                end
                task.wait(0.3)
                game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX", Text="✅ Palhaço guardado!", Duration=3})
            end
        else
            game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX", Text="Não achei tua base!", Duration=2})
        end
    end
end)

-- reconecta quando respawna
LP.CharacterAdded:Connect(function(char)
    task.wait(1)
    char.ChildAdded:Connect(function(child)
        if child:IsA("Tool") and flyEnabled then
            task.wait(0.1)
            local myPlot = getMyPlot()
            if myPlot then
                voarAte(myPlot:GetPivot().Position)
                task.wait(0.3)
                if kitarEnabled then
                    local tool = LP.Character:FindFirstChildWhichIsA("Tool")
                    if tool then tool.Parent = LP.Backpack end
                end
            end
        end
    end)
end)

game.StarterGui:SetCore("SendNotification",{Title="MIIIGUEX V21", Text="Ativa o VOAR PRA BASE e pega o palhaço na mão!", Duration=4})
