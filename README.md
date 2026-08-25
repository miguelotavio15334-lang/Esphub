-- MIIIGUEX V26 - LISTA 75K+ 93.7K 150K 175K 200K 225K 400K 500K 600K | P TP | R OFF | RED
getgenv().MIIIGUEX_DATA = {autoHop=false,noclip=true,autoK=true,guiHidden=true,savedTP=nil,speed=true,tpAuto=true}
getgenv().Visitados = getgenv().Visitados or {}

local LP = game.Players.LocalPlayer
local TS = game:GetService("TeleportService")
local RS = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local HS = game:GetService("HttpService")
local Plots = workspace:WaitForChild("Plots")
local SG = game.StarterGui

local function notify(t,m,d) pcall(function() SG:SetCore("SendNotification",{Title=t, Text=m, Duration=d or 3}) end) end

local function ServerHop()
    notify("🔄 PROCURANDO","BUSCANDO PALHAÇO 75K+",2)
    table.insert(getgenv().Visitados, game.JobId)
    local PlaceId = game.PlaceId
    local servers = {}
    local ok, result = pcall(function()
        return HS:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..PlaceId.."/servers/Public?sortOrder=Asc&limit=100"))
    end)
    if ok and result and result.data then
        for _,s in pairs(result.data) do
            if s.id ~= game.JobId and s.playing < s.maxPlayers then
                local jaFoi = false
                for _,v in pairs(getgenv().Visitados) do if v==s.id then jaFoi=true break end end
                if not jaFoi then table.insert(servers, s.id) end
            end
        end
        if #servers > 0 then
            local escolhido = servers[math.random(1,#servers)]
            pcall(function() TS:TeleportToPlaceInstance(PlaceId, escolhido, LP) end)
            task.wait(0.5)
        end
    end
    if #getgenv().Visitados > 35 then getgenv().Visitados = {} end
    pcall(function() TS:Teleport(PlaceId, LP) end)
end

local queueteleport = queue_on_teleport or (syn and syn.queue_on_teleport)
if queueteleport then
    queueteleport([[
        getgenv().MIIIGUEX_DATA = {autoHop=false,noclip=true,autoK=true,guiHidden=true,savedTP=nil,speed=true,tpAuto=true}
        getgenv().Visitados = getgenv().Visitados or {}
        wait(3)
        loadstring(game:HttpGet("https://pastebin.com/raw/SEU_LINK_AQUI"))()
    ]])
end

pcall(function() LP.PlayerGui:FindFirstChild("MIIIGUEX_HUB"):Destroy() end)
local gui = Instance.new("ScreenGui", LP.PlayerGui) gui.Name = "MIIIGUEX_HUB" gui.ResetOnSpawn = false
local main = Instance.new("Frame", gui) main.Size = UDim2.new(0,380,0,650) main.Position = UDim2.new(0.5,-190,0.5,-325) main.BackgroundColor3 = Color3.fromRGB(120,15,15) main.Active=true main.Draggable=true Instance.new("UICorner", main).CornerRadius = UDim.new(0,18)
local stroke = Instance.new("UIStroke", main) stroke.Color = Color3.fromRGB(255,0,0) stroke.Thickness = 2
local openBtn = Instance.new("TextButton", gui) openBtn.Size=UDim2.new(0,60,0,60) openBtn.Position=UDim2.new(0,15,0.5,-30) openBtn.Text="M" openBtn.Visible=true openBtn.BackgroundColor3=Color3.fromRGB(120,15,15) openBtn.TextColor3=Color3.new(1,1,1) openBtn.Font=Enum.Font.GothamBold openBtn.TextSize=24 Instance.new("UICorner", openBtn).CornerRadius=UDim.new(0,30) local stroke2 = Instance.new("UIStroke", openBtn) stroke2.Thickness=2 stroke2.Color=Color3.fromRGB(255,0,0)

local function btn(txt,y,col) local b=Instance.new("TextButton", main) b.Position=UDim2.new(0,15,0,y) b.Size=UDim2.new(1,-30,0,44) b.Text=txt b.BackgroundColor3=col or Color3.fromRGB(70,15,15) b.TextColor3=Color3.new(1,1,1) b.Font=Enum.Font.GothamBold b.TextSize=12 Instance.new("UICorner", b).CornerRadius=UDim.new(0,12) return b end

local bSpeed=btn("SPEED 150: ON",50,Color3.fromRGB(35,85,55))
local bNo=btn("NoClip: ON",98,Color3.fromRGB(35,85,55))
local bKitar=btn("Auto Kitar [E]: ON",146,Color3.fromRGB(35,85,55))
local bTP=btn("Q AUTO AO PEGAR: ON",194,Color3.fromRGB(35,85,55))
local bSave=btn("💾 SALVAR BASE [X]",242)
local bUse=btn("📍 USAR TP [Q]",290,Color3.fromRGB(60,20,20))
local bPalhaco=btn("🤡 TP PALHAÇO [P] 75K+",338,Color3.fromRGB(120,30,30))
local bAutoHop=btn("AUTO HOP [R]: OFF",386, Color3.fromRGB(45,47,55))
local bClose=btn("FECHAR [M] | VISITADOS: "..#getgenv().Visitados,434,Color3.fromRGB(80,20,20))

local noclip=true local autoK=true local tpAuto=true local speedOn=true local autoHop=false
local savedCFrame=nil

main.Visible = false openBtn.Visible = true

local function getBase() for _,p in pairs(Plots:GetChildren()) do if p:FindFirstChild("Owner") and p.Owner.Value==LP.Name then return p:GetPivot().Position+Vector3.new(0,5,0) end end end
local function toggleGUI() main.Visible=not main.Visible openBtn.Visible=not main.Visible end
local function salvarPos() local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") if hrp then savedCFrame=hrp.CFrame getgenv().MIIIGUEX_DATA.savedTP={savedCFrame:GetComponents()} bSave.Text="💾 X SALVO!" bSave.BackgroundColor3=Color3.fromRGB(35,85,55) end end
local function usarPos() if savedCFrame then local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") if hrp then hrp.CFrame=savedCFrame end end end

local function temPalhaco100k()
    local melhor, valor, dono = nil, 0, ""
    for _,plot in pairs(Plots:GetChildren()) do
        if plot:FindFirstChild("Owner") and plot.Owner.Value~=LP.Name then
            for _,m in pairs(plot:GetChildren()) do
                if m:IsA("Model") and m.PrimaryPart and m:FindFirstChild("PricePerSecond") then
                    local nome = string.lower(m.Name)
                    local ePalhaco = string.find(nome,"clown") or string.find(nome,"palhaco") or string.find(nome,"jester") or string.find(nome,"palha") or string.find(nome,"circus")
                    if ePalhaco then
                        local v = m.PricePerSecond.Value
                        -- LISTA QUE TU QUER: 75K 93.7K 150K 175K 200K 225K 400K 500K 600K
                        if v >= 73000 then
                            if v > valor then
                                valor = v
                                melhor = m
                                dono = plot.Owner.Value
                            end
                        end
                    end
                end
            end
        end
    end
    return melhor, valor, dono
end

local function tpPalhaco()
    local best,val,dono = temPalhaco100k()
    if best then
        local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
        if hrp then hrp.CFrame=best.PrimaryPart.CFrame+Vector3.new(0,0,3) notify("🤡 TP [P]","$"..val.." | "..dono,2) end
    else
        notify("🤡 SEM PALHAÇO","Nenhum 75K+ aqui",2)
    end
end

local function ativarAutoHop()
    autoHop = not autoHop
    bAutoHop.Text="AUTO HOP [R]: "..(autoHop and "ON - CAÇANDO 75K+" or "OFF")
    bAutoHop.BackgroundColor3=autoHop and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55)
    if autoHop then notify("🤡 CAÇADOR","Procurando palhaço 75K+",3) else notify("AUTO HOP","OFF",2) end
end

bSpeed.MouseButton1Click:Connect(function() speedOn=not speedOn bSpeed.Text=speedOn and "SPEED 150: ON" or "SPEED 16: OFF" bSpeed.BackgroundColor3=speedOn and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bNo.MouseButton1Click:Connect(function() noclip=not noclip bNo.Text=noclip and "NoClip: ON" or "NoClip: OFF" bNo.BackgroundColor3=noclip and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bKitar.MouseButton1Click:Connect(function() autoK=not autoK bKitar.Text=autoK and "Auto Kitar [E]: ON" or "Auto Kitar [E]: OFF" bKitar.BackgroundColor3=autoK and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bTP.MouseButton1Click:Connect(function() tpAuto=not tpAuto bTP.Text=tpAuto and "Q AUTO AO PEGAR: ON" or "Q AUTO AO PEGAR: OFF" bTP.BackgroundColor3=tpAuto and Color3.fromRGB(35,85,55) or Color3.fromRGB(45,47,55) end)
bSave.MouseButton1Click:Connect(salvarPos) bUse.MouseButton1Click:Connect(usarPos)
bPalhaco.MouseButton1Click:Connect(tpPalhaco)
bAutoHop.MouseButton1Click:Connect(ativarAutoHop)
bClose.MouseButton1Click:Connect(function() if UIS:IsKeyDown(Enum.KeyCode.LeftControl) then getgenv().Visitados={} bClose.Text="FECHAR [M] | VISITADOS: 0" notify("LIMPO","Lista limpa!",2) else toggleGUI() end end) openBtn.MouseButton1Click:Connect(toggleGUI)

UIS.InputBegan:Connect(function(i,gp)
    if gp then return end
    if i.KeyCode==Enum.KeyCode.M then toggleGUI()
    elseif i.KeyCode==Enum.KeyCode.R then ativarAutoHop()
    elseif i.KeyCode==Enum.KeyCode.P then tpPalhaco()
    elseif i.KeyCode==Enum.KeyCode.X then salvarPos()
    elseif i.KeyCode==Enum.KeyCode.Q then usarPos()
    elseif i.KeyCode==Enum.KeyCode.L then getgenv().Visitados={} bClose.Text="FECHAR [M] | VISITADOS: 0" notify("LIMPO","Visitados zerado",2)
    elseif i.KeyCode==Enum.KeyCode.E and autoK then
        pcall(function() local t=LP.Character:FindFirstChildWhichIsA("Tool") if t then local n=string.lower(t.Name) if not (string.find(n,"cloak") or string.find(n,"invis") or string.find(n,"cape") or string.find(n,"ghost")) then t.Parent=LP.Backpack end end end)
    end
end)

RS.Stepped:Connect(function() pcall(function() local char = LP.Character if not char then return end if noclip then for _,v in pairs(char:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide=false end end end if speedOn then local hum=char:FindFirstChildOfClass("Humanoid") if hum and hum.WalkSpeed~=150 then hum.WalkSpeed=150 end end end) end)

task.spawn(function()
    task.wait(3)
    local basePos=getBase() if basePos then savedCFrame=CFrame.new(basePos) bSave.Text="💾 X AUTO: BASE SALVA" bSave.BackgroundColor3=Color3.fromRGB(35,85,55) end
    pcall(function() LP.Character.Humanoid.WalkSpeed=150 end)
end)

task.spawn(function()
    local tempoSem100k = 0
    while true do task.wait(0.15)
        pcall(function()
            local char=LP.Character if not char then return end local hrp=char:FindFirstChild("HumanoidRootPart") if not hrp then return end
            local tool=char:FindFirstChildWhichIsA("Tool")
            if tool then
                local n=string.lower(tool.Name) if string.find(n,"cloak") or string.find(n,"invis") or string.find(n,"cape") or string.find(n,"ghost") then return end
                if tpAuto and savedCFrame then hrp.CFrame=savedCFrame task.wait(0.35) local t=char:FindFirstChildWhichIsA("Tool") if t then local nn=string.lower(t.Name) if not (string.find(nn,"cloak") or string.find(nn,"invis") or string.find(nn,"cape") or string.find(nn,"ghost")) then t.Parent=LP.Backpack end end end
            else
                local best,val,dono = temPalhaco100k()
                if best then
                    tempoSem100k = 0
                    bAutoHop.Text="🤡 TRAVADO! $"..val.." | "..dono
                    bAutoHop.BackgroundColor3=Color3.fromRGB(255,200,0)
                    hrp.CFrame=best.PrimaryPart.CFrame+Vector3.new(0,0,2) task.wait(0.15) firetouchinterest(hrp,best.PrimaryPart,0) firetouchinterest(hrp,best.PrimaryPart,1) for _,d in pairs(best:GetDescendants()) do if d:IsA("ProximityPrompt") then fireproximityprompt(d) end end
                else
                    if autoHop then
                        tempoSem100k = tempoSem100k + 0.15
                        bAutoHop.Text="🔍 PROCURANDO 75K+ ("..string.format("%.1f",tempoSem100k).."s) V:"..#getgenv().Visitados
                        bAutoHop.BackgroundColor3=Color3.fromRGB(255,50,50)
                        if tempoSem100k > 2 then ServerHop() tempoSem100k = 0 task.wait(3) end
                    else
                        bAutoHop.Text="AUTO HOP [R]: OFF"
                        bAutoHop.BackgroundColor3=Color3.fromRGB(45,47,55)
                        tempoSem100k = 0
                    end
                    bClose.Text="FECHAR [M] | VISITADOS: "..#getgenv().Visitados
                end
            end
        end)
    end
end)

notify("MIIIGUEX V26","75K 93.7K 150K 175K 200K 225K 400K 500K 600K | P TP | E kitar",5)
