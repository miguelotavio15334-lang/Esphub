-- MIIIGUEX V24 - R FORÇADO + ANTI-LOOP + DELTA FIX
getgenv().MIIIGUEX_DATA = getgenv().MIIIGUEX_DATA or {autoHop=false,noclip=true,autoK=true,guiHidden=true,savedTP=nil,speed=true,tpAuto=true}
getgenv().MIIIGUEX_DATA.guiHidden = true
getgenv().Visitados = getgenv().Visitados or {}

local LP = game.Players.LocalPlayer
local TS = game:GetService("TeleportService")
local RS = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local HS = game:GetService("HttpService")
local Plots = workspace:WaitForChild("Plots")
local SG = game.StarterGui

-- HOP FORÇADO QUE TROCA SIM
local function ServerHop()
    SG:SetCore("SendNotification",{Title="AUTO HOP", Text="TROCANDO DE SERVER AGORA!", Duration=2})
    task.wait(0.2)
    local PlaceId = game.PlaceId
    table.insert(getgenv().Visitados, game.JobId)

    -- JEITO 1: NORMAL (MAIS RAPIDO)
    local ok = pcall(function() TS:Teleport(PlaceId, LP) end)
    if ok then return end
    task.wait(0.5)
    -- JEITO 2: FALLBACK
    pcall(function() TS:TeleportToPlaceInstance(PlaceId, game.JobId, LP) end)
    task.wait(0.5)
    pcall(function() TS:Teleport(PlaceId) end)
end

local queueteleport = queue_on_teleport or (syn and syn.queue_on_teleport)
if queueteleport then
    queueteleport([[
        getgenv().MIIIGUEX_DATA = {autoHop=true,noclip=true,autoK=true,guiHidden=true,savedTP=nil,speed=true,tpAuto=true}
        wait(3)
        loadstring(game:HttpGet("https://pastebin.com/raw/SEU_LINK_AQUI"))()
    ]])
end

pcall(function() LP.PlayerGui:FindFirstChild("MIIIGUEX_HUB"):Destroy() end)
local gui = Instance.new("ScreenGui", LP.PlayerGui) gui.Name = "MIIIGUEX_HUB" gui.ResetOnSpawn = false
local main = Instance.new("Frame", gui) main.Size = UDim2.new(0,380,0,620) main.Position = UDim2.new(0.5,-190,0.5,-310) main.BackgroundColor3 = Color3.fromRGB(24,24,28) main.Active=true main.Draggable=true Instance.new("UICorner", main).CornerRadius = UDim.new(0,18)
local openBtn = Instance.new("TextButton", gui) openBtn.Size=UDim2.new(0,60,0,60) openBtn.Position=UDim2.new(0,15,0.5,-30) openBtn.Text="M" openBtn.Visible=true openBtn.BackgroundColor3=Color3.fromRGB(25,25,25) openBtn.TextColor3=Color3.new(1,1,1) openBtn.Font=Enum.Font.GothamBold openBtn.TextSize=24 Instance.new("UICorner", openBtn).CornerRadius=UDim.new(0,30) local stroke = Instance.new("UIStroke", openBtn) stroke.Thickness=2 stroke.Color=Color3.fromRGB(0,255,100)

local function btn(txt,y,col) local b=Instance.new("TextButton", main) b.Position=UDim2.new(0,15,0,y) b.Size=UDim2.new(1,-30,0,44) b.Text=txt b.BackgroundColor3=col or Color3.fromRGB(45,47,55) b.TextColor3=Color3.new(1,1,1) b.Font=Enum.Font.GothamBold b.TextSize=12 Instance.new("UICorner", b).CornerRadius=UDim.new(0,12) return b end

local bSpeed=btn("SPEED 150: ON",50,Color3.fromRGB(35,85,55))
local bNo=btn("NoClip: ON",98,Color3.fromRGB(35,85,55))
local bKitar=btn("Auto Kitar [K]: ON",146,Color3.fromRGB(35,85,55))
local bTP=btn("Q AUTO AO PEGAR: ON",194,Color3.fromRGB(35,85,55))
local bSave=btn("💾 SALVAR BASE [X]",242)
local bUse=btn("📍 USAR TP [Q]",290,Color3.fromRGB(60,60,90))
local bAutoHop=btn("AUTO HOP [R]: "..(getgenv().MIIIGUEX_DATA.autoHop and "ON" or "OFF"),338, getgenv().MIIIGUEX_DATA.autoHop and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55))
local bClose=btn("FECHAR [M]",386,Color3.fromRGB(80,40,40))

local noclip=true local autoK=true local tpAuto=true local speedOn=true local autoHop=getgenv().MIIIGUEX_DATA.autoHop or false
local savedCFrame=getgenv().MIIIGUEX_DATA.savedTP and CFrame.new(unpack(getgenv().MIIIGUEX_DATA.savedTP)) or nil

main.Visible = false openBtn.Visible = true
if savedCFrame then bSave.Text="💾 X: SALVO!" bSave.BackgroundColor3=Color3.fromRGB(35,85,55) end

local function getBase() for _,p in pairs(Plots:GetChildren()) do if p:FindFirstChild("Owner") and p.Owner.Value==LP.Name then return p:GetPivot().Position+Vector3.new(0,5,0) end end end
local function toggleGUI() main.Visible=not main.Visible openBtn.Visible=not main.Visible end
local function salvarPos() local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") if hrp then savedCFrame=hrp.CFrame getgenv().MIIIGUEX_DATA.savedTP={savedCFrame:GetComponents()} bSave.Text="💾 X SALVO!" bSave.BackgroundColor3=Color3.fromRGB(35,85,55) end end
local function usarPos() if savedCFrame then local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") if hrp then hrp.CFrame=savedCFrame end end end

local function temPalhaco100k()
    local melhor, valor = nil, 0
    for _,plot in pairs(Plots:GetChildren()) do
        if plot:FindFirstChild("Owner") and plot.Owner.Value~=LP.Name then
            for _,m in pairs(plot:GetChildren()) do
                if m:IsA("Model") and m.PrimaryPart and m:FindFirstChild("PricePerSecond") then
                    if m.PricePerSecond.Value >= 100000 and m.PricePerSecond.Value > valor then valor = m.PricePerSecond.Value melhor = m end
                end
            end
        end
    end
    return melhor, valor
end

local function ativarAutoHop()
    autoHop = not autoHop
    getgenv().MIIIGUEX_DATA.autoHop = autoHop
    bAutoHop.Text="AUTO HOP [R]: "..(autoHop and "ON" or "OFF")
    bAutoHop.BackgroundColor3=autoHop and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55)
    if autoHop then
        SG:SetCore("SendNotification",{Title="AUTO HOP [R]", Text="ATIVADO! Vai trocar em 3s se não achar 100K+", Duration=3})
    else
        SG:SetCore("SendNotification",{Title="AUTO HOP [R]", Text="Desligado", Duration=2})
    end
end

bSpeed.MouseButton1Click:Connect(function() speedOn=not speedOn getgenv().MIIIGUEX_DATA.speed=speedOn bSpeed.Text=speedOn and "SPEED 150: ON" or "SPEED 16: OFF" bSpeed.BackgroundColor3=speedOn and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bNo.MouseButton1Click:Connect(function() noclip=not noclip bNo.Text=noclip and "NoClip: ON" or "NoClip: OFF" bNo.BackgroundColor3=noclip and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bKitar.MouseButton1Click:Connect(function() autoK=not autoK bKitar.Text=autoK and "Auto Kitar [K]: ON" or "Auto Kitar [K]: OFF" bKitar.BackgroundColor3=autoK and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bTP.MouseButton1Click:Connect(function() tpAuto=not tpAuto bTP.Text=tpAuto and "Q AUTO AO PEGAR: ON" or "Q AUTO AO PEGAR: OFF" bTP.BackgroundColor3=tpAuto and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bSave.MouseButton1Click:Connect(salvarPos) bUse.MouseButton1Click:Connect(usarPos)
bAutoHop.MouseButton1Click:Connect(ativarAutoHop)
bClose.MouseButton1Click:Connect(toggleGUI) openBtn.MouseButton1Click:Connect(toggleGUI)

UIS.InputBegan:Connect(function(i,gp)
    if gp then return end
    if i.KeyCode==Enum.KeyCode.M then toggleGUI()
    elseif i.KeyCode==Enum.KeyCode.R then ativarAutoHop()
    elseif i.KeyCode==Enum.KeyCode.X then salvarPos()
    elseif i.KeyCode==Enum.KeyCode.Q then usarPos()
    elseif i.KeyCode==Enum.KeyCode.K and autoK then
        pcall(function() local t=LP.Character:FindFirstChildWhichIsA("Tool") if t then local n=string.lower(t.Name) if not (string.find(n,"cloak") or string.find(n,"invis") or string.find(n,"cape") or string.find(n,"ghost")) then t.Parent=LP.Backpack end end end)
    end
end)

RS.Stepped:Connect(function() pcall(function() local char = LP.Character if not char then return end if noclip then for _,v in pairs(char:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide=false end end end if speedOn then local hum=char:FindFirstChildOfClass("Humanoid") if hum and hum.WalkSpeed~=150 then hum.WalkSpeed=150 end end end) end)

task.spawn(function()
    task.wait(3)
    if getgenv().MIIIGUEX_DATA.savedTP then savedCFrame=CFrame.new(unpack(getgenv().MIIIGUEX_DATA.savedTP)) local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") if hrp then hrp.CFrame=savedCFrame end
    else local basePos=getBase() if basePos then savedCFrame=CFrame.new(basePos) getgenv().MIIIGUEX_DATA.savedTP={savedCFrame:GetComponents()} bSave.Text="💾 X AUTO: BASE SALVA" bSave.BackgroundColor3=Color3.fromRGB(35,85,55) end end
    pcall(function() LP.Character.Humanoid.WalkSpeed=150 end)
end)

task.spawn(function()
    local tempoSem100k = 0
    while true do task.wait(0.1)
        pcall(function()
            local char=LP.Character if not char then return end local hrp=char:FindFirstChild("HumanoidRootPart") if not hrp then return end
            local tool=char:FindFirstChildWhichIsA("Tool")
            if tool then
                local n=string.lower(tool.Name) if string.find(n,"cloak") or string.find(n,"invis") or string.find(n,"cape") or string.find(n,"ghost") then return end
                if tpAuto and savedCFrame then hrp.CFrame=savedCFrame task.wait(0.35) local t=char:FindFirstChildWhichIsA("Tool") if t then local nn=string.lower(t.Name) if not (string.find(nn,"cloak") or string.find(nn,"invis") or string.find(nn,"cape") or string.find(nn,"ghost")) then t.Parent=LP.Backpack end end end
            else
                local best,val = temPalhaco100k()
                if best then
                    tempoSem100k = 0
                    hrp.CFrame=best.PrimaryPart.CFrame+Vector3.new(0,0,2) task.wait(0.15) firetouchinterest(hrp,best.PrimaryPart,0) firetouchinterest(hrp,best.PrimaryPart,1) for _,d in pairs(best:GetDescendants()) do if d:IsA("ProximityPrompt") then fireproximityprompt(d) end end
                else
                    if autoHop then
                        tempoSem100k = tempoSem100k + 0.1
                        if tempoSem100k > 3 then
                            ServerHop()
                            tempoSem100k = 0
                            task.wait(5)
                        end
                    end
                end
            end
        end)
    end
end)

SG:SetCore("SendNotification",{Title="MIIIGUEX V24", Text="Aperta R pra ativar AUTO HOP FORÇADO!", Duration=5})
