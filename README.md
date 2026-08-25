-- MIIIGUEX V28 - VISUAL IGUAL FOTO - TOGGLES VERDE
pcall(function() game.Players.LocalPlayer.PlayerGui:FindFirstChild("MIIIGUEX_HUB"):Destroy() end)

local LP = game.Players.LocalPlayer
local Plots = workspace:WaitForChild("Plots")
local TS = game:GetService("TeleportService")
local RS = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local HS = game:GetService("HttpService")
local SG = game.StarterGui

getgenv().Visitados = getgenv().Visitados or {}
local function notify(t,m,d) pcall(function() SG:SetCore("SendNotification",{Title=t,Text=m,Duration=d or 3}) end) end

-- GUI BASE
local gui = Instance.new("ScreenGui", LP.PlayerGui) gui.Name = "MIIIGUEX_HUB" gui.ResetOnSpawn = false
local main = Instance.new("Frame", gui) main.Size = UDim2.new(0,380,0,680) main.Position = UDim2.new(0.5,-190,0.5,-340) main.BackgroundColor3 = Color3.fromRGB(10,10,12) main.Active=true main.Draggable=true
Instance.new("UICorner", main).CornerRadius = UDim.new(0,20)
local strokeMain = Instance.new("UIStroke", main) strokeMain.Color = Color3.fromRGB(255,30,30) strokeMain.Thickness = 2 strokeMain.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

-- HEADER
local header = Instance.new("Frame", main) header.Size = UDim2.new(1,-4,0,85) header.Position = UDim2.new(0,2,0,2) header.BackgroundColor3 = Color3.fromRGB(20,10,10) Instance.new("UICorner", header).CornerRadius = UDim.new(0,18)
local title = Instance.new("TextLabel", header) title.Size = UDim2.new(1,-70,1,0) title.Position = UDim2.new(0,60,0,0) title.Text="MIIIGUEX V28" title.TextColor3=Color3.fromRGB(255,50,50) title.Font=Enum.Font.GothamBlack title.TextSize=32 title.BackgroundTransparency=1 title.TextXAlignment=Enum.TextXAlignment.Left
local skull = Instance.new("TextLabel", header) skull.Size=UDim2.new(0,50,0,50) skull.Position=UDim2.new(0,8,0,15) skull.Text="💀" skull.TextSize=35 skull.BackgroundTransparency=1
local status = Instance.new("TextLabel", header) status.Size=UDim2.new(0,120,0,35) status.Position=UDim2.new(1,-125,0,5) status.Text="STATUS: INJECTED\n● ONLINE\nv28.0.1" status.TextColor3=Color3.fromRGB(0,255,100) status.Font=Enum.Font.Code status.TextSize=10 status.BackgroundTransparency=1

local sub = Instance.new("TextLabel", main) sub.Size=UDim2.new(1,0,0,20) sub.Position=UDim2.new(0,0,0,90) sub.Text="EXPLOIT MODULES • 14 OPTIONS • BUILT FOR ROBLOX" sub.TextColor3=Color3.fromRGB(180,30,30) sub.Font=Enum.Font.GothamBold sub.TextSize=10 sub.BackgroundTransparency=1

-- CONTAINER DOS BOTOES
local container = Instance.new("Frame", main) container.Size=UDim2.new(1,-10,1,-125) container.Position=UDim2.new(0,5,0,115) container.BackgroundColor3=Color3.fromRGB(15,15,17) Instance.new("UICorner", container).CornerRadius=UDim.new(0,14)
local layout = Instance.new("UIListLayout", container) layout.Padding=UDim.new(0,6) layout.SortOrder=Enum.SortOrder.LayoutOrder
local pad = Instance.new("UIPadding", container) pad.PaddingTop=UDim.new(0,8) pad.PaddingBottom=UDim.new(0,8) pad.PaddingLeft=UDim.new(0,8) pad.PaddingRight=UDim.new(0,8)

-- FUNCAO TOGGLE IGUAL DA FOTO
local toggles = {}
local function createToggle(nome, ligado)
    local frame = Instance.new("Frame", container) frame.Size=UDim2.new(1,0,0,40) frame.BackgroundColor3=Color3.fromRGB(35,15) Instance.new("UICorner", frame).CornerRadius=UDim.new(0,20)
    local s = Instance.new("UIStroke", frame) s.Color=Color3.fromRGB(80,20,20) s.Thickness=1
    local label = Instance.new("TextLabel", frame) label.Size=UDim2.new(1,-90,1,0) label.Position=UDim2.new(0,15,0,0) label.Text=nome label.TextColor3=Color3.new(1,1,1) label.Font=Enum.Font.GothamBold label.TextSize=13 label.BackgroundTransparency=1 label.TextXAlignment=Enum.TextXAlignment.Left

    local toggleBG = Instance.new("Frame", frame) toggleBG.Size=UDim2.new(0,65,0,30) toggleBG.Position=UDim2.new(1,-75,0.5,-15) toggleBG.BackgroundColor3=ligado and Color3.fromRGB(60,120,60) or Color3.fromRGB(80,80,80) Instance.new("UICorner", toggleBG).CornerRadius=UDim.new(1,0)
    local circle = Instance.new("Frame", toggleBG) circle.Size=UDim2.new(0,26,0,26) circle.Position=ligado and UDim2.new(1,-28,0.5,-13) or UDim2.new(0,2,0.5,-13) circle.BackgroundColor3=ligado and Color3.fromRGB(100,255,120) or Color3.fromRGB(180,180,180) Instance.new("UICorner", circle).CornerRadius=UDim.new(1,0)
    local onTxt = Instance.new("TextLabel", toggleBG) onTxt.Size=UDim2.new(0,25,1,0) onTxt.Position=UDim2.new(0,5,0,0) onTxt.Text=ligado and "ON" or "OFF" onTxt.TextColor3=ligado and Color3.fromRGB(180,255,180) or Color3.fromRGB(200,200,200) onTxt.Font=Enum.Font.GothamBold onTxt.TextSize=11 onTxt.BackgroundTransparency=1 onTxt.Visible=ligado
    onTxt.TextXAlignment=Enum.TextXAlignment.Left
    if not ligado then onTxt.Position=UDim2.new(1,-30,0,0) onTxt.Text="OFF" onTxt.Visible=true end

    local btn = Instance.new("TextButton", frame) btn.Size=UDim2.new(1,0,1,0) btn.BackgroundTransparency=1 btn.Text=""

    local state = ligado
    btn.MouseButton1Click:Connect(function()
        state = not state
        toggleBG.BackgroundColor3=state and Color3.fromRGB(60,120,60) or Color3.fromRGB(80,80,80)
        circle.Position=state and UDim2.new(1,-28,0.5,-13) or UDim2.new(0,2,0.5,-13)
        circle.BackgroundColor3=state and Color3.fromRGB(100,255,120) or Color3.fromRGB(180,180,180)
        onTxt.Text=state and "ON" or "OFF"
        onTxt.Visible=true
        if state then onTxt.Position=UDim2.new(0,5,0,0) else onTxt.Position=UDim2.new(1,-30,0,0) end
        if toggles[nome] then toggles[nome].callback(state) end
    end)
    return {frame=frame, getState=function() return state end, setState=function(v) state=v toggleBG.BackgroundColor3=v and Color3.fromRGB(60,120,60) or Color3.fromRGB(80,80,80) circle.Position=v and UDim2.new(1,-28,0.5,-13) or UDim2.new(0,2,0.5,-13) end}
end

-- BOTAO FECHAR VERMELHO
local function createFechar()
    local frame = Instance.new("Frame", container) frame.Size=UDim2.new(1,0,0,45) frame.BackgroundColor3=Color3.fromRGB(180,20,20) Instance.new("UICorner", frame).CornerRadius=UDim.new(0,12)
    local label = Instance.new("TextLabel", frame) label.Size=UDim2.new(1,0,1,0) label.Text="FECHAR [M]" label.TextColor3=Color3.new(1,1,1) label.Font=Enum.Font.GothamBlack label.TextSize=18 label.BackgroundTransparency=1
    local btn = Instance.new("TextButton", frame) btn.Size=UDim2.new(1,0,1,0) btn.BackgroundTransparency=1 btn.Text=""
    btn.MouseButton1Click:Connect(function() main.Visible=false openBtn.Visible=true end)
end

-- BOTAO ABRIR M
local openBtn = Instance.new("TextButton", gui) openBtn.Size=UDim2.new(0,60,0,60) openBtn.Position=UDim2.new(0,15,0.5,-30) openBtn.Text="M" openBtn.Visible=false openBtn.BackgroundColor3=Color3.fromRGB(20,10,10) openBtn.TextColor3=Color3.fromRGB(255,50,50) openBtn.Font=Enum.Font.GothamBlack openBtn.TextSize=28 Instance.new("UICorner", openBtn).CornerRadius=UDim.new(0,30) local st=Instance.new("UIStroke", openBtn) st.Color=Color3.fromRGB(255,30,30) st.Thickness=2
openBtn.MouseButton1Click:Connect(function() main.Visible=true openBtn.Visible=false end)

-- VARIAVEIS
local noclip=true local autoK=true local tpAuto=true local speedOn=true local autoHop=false
local autoInvis=true local antiHit=true local autoClick=true local espOn=true local autoTrap=true
local savedCFrame=nil
local highlights={}

local function getBase() for _,p in pairs(Plots:GetChildren()) do if p:FindFirstChild("Owner") and p.Owner.Value==LP.Name then return p:GetPivot().Position+Vector3.new(0,5,0) end end end
local function salvarPos() local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") if hrp then savedCFrame=hrp.CFrame notify("💾 SALVO","Base salva!",2) end end
local function usarPos() if savedCFrame then local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") if hrp then hrp.CFrame=savedCFrame end end end

local function temMelhorPalhaco()
    local melhor, valor, dono = nil, 0, ""
    for _,plot in pairs(Plots:GetChildren()) do
        if plot:FindFirstChild("Owner") and plot.Owner.Value~=LP.Name then
            for _,m in pairs(plot:GetChildren()) do
                if m:IsA("Model") and m.PrimaryPart and m:FindFirstChild("PricePerSecond") then
                    local nome = string.lower(m.Name)
                    local ePalhaco = string.find(nome,"clown") or string.find(nome,"palhaco") or string.find(nome,"jester") or string.find(nome,"palha") or string.find(nome,"circus")
                    if ePalhaco and m.PricePerSecond.Value > valor then valor=m.PricePerSecond.Value melhor=m dono=plot.Owner.Value end
                end
            end
        end
    end
    return melhor, valor, dono
end

local function criarESP(model, valor, dono)
    if not model.PrimaryPart then return end
    if model.PrimaryPart:FindFirstChild("MIIIGUEX_ESP") then model.PrimaryPart.MIIIGUEX_ESP:Destroy() end
    local bb = Instance.new("BillboardGui", model.PrimaryPart) bb.Name="MIIIGUEX_ESP" bb.Size=UDim2.new(0,200,0,50) bb.StudsOffset=Vector3.new(0,4,0) bb.AlwaysOnTop=true
    local txt = Instance.new("TextLabel", bb) txt.Size=UDim2.new(1,0,1,0) txt.BackgroundTransparency=1 txt.Text="🤡 $"..valor.." | "..dono txt.TextColor3=Color3.fromRGB(255,255,0) txt.TextStrokeTransparency=0.2 txt.Font=Enum.Font.GothamBold txt.TextSize=14
    local hl = Instance.new("Highlight", model) hl.Name="MIIIGUEX_HL" hl.FillColor=Color3.fromRGB(255,0,0) hl.OutlineColor=Color3.fromRGB(255,255,0) hl.FillTransparency=0.5
end

local function tpPalhaco()
    local best,val,dono = temMelhorPalhaco()
    if best then local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") if hrp then hrp.CFrame=best.PrimaryPart.CFrame+Vector3.new(0,0,3) notify("🤡 TP [P]","$"..val.." | "..dono,2) end
    else notify("🤡 SEM PALHAÇO","Nenhum palhaço",2) end
end

local function ServerHop()
    notify("🔄 HOP","Procurando palhaço",2) table.insert(getgenv().Visitados, game.JobId)
    local PlaceId=game.PlaceId local servers={} local ok,res=pcall(function() return HS:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..PlaceId.."/servers/Public?sortOrder=Asc&limit=100")) end)
    if ok and res and res.data then for _,s in pairs(res.data) do if s.id~=game.JobId and s.playing<s.maxPlayers then local ja=false for _,v in pairs(getgenv().Visitados) do if v==s.id then ja=true break end end if not ja then table.insert(servers,s.id) end end end if #servers>0 then pcall(function() TS:TeleportToPlaceInstance(PlaceId, servers[math.random(1,#servers)], LP) end) task.wait(0.5) end end if #getgenv().Visitados>35 then getgenv().Visitados={} end pcall(function() TS:Teleport(PlaceId, LP) end)
end

-- CRIAR TOGGLES IGUAL FOTO
toggles["SPEED 150 ON"]={callback=function(v) speedOn=v end}
createToggle("SPEED 150 ON", true)
toggles["NoClip ON"]={callback=function(v) noclip=v end}
createToggle("NoClip ON", true)
toggles["Auto Kitar [E]"]={callback=function(v) autoK=v end}
createToggle("Auto Kitar [E]", true)
toggles["Q AUTO AO PEGAR"]={callback=function(v) tpAuto=v end}
createToggle("Q AUTO AO PEGAR", true)
toggles["AUTO INVIS AO PEGAR"]={callback=function(v) autoInvis=v end}
createToggle("AUTO INVIS AO PEGAR", true)
toggles["ANTI-HIT TASER"]={callback=function(v) antiHit=v end}
createToggle("ANTI-HIT TASER", true)
toggles["AUTO CLICK TURBO"]={callback=function(v) autoClick=v end}
createToggle("AUTO CLICK TURBO", true)
toggles["ESP PALHAÇO"]={callback=function(v) espOn=v if not v then for _,p in pairs(Plots:GetChildren()) do for _,m in pairs(p:GetChildren()) do if m:FindFirstChild("MIIIGUEX_HL") then m.MIIIGUEX_HL:Destroy() end if m.PrimaryPart and m.PrimaryPart:FindFirstChild("MIIIGUEX_ESP") then m.PrimaryPart.MIIIGUEX_ESP:Destroy() end end end end end}
createToggle("ESP PALHAÇO", true)
toggles["AUTO TRAP LASER"]={callback=function(v) autoTrap=v end}
createToggle("AUTO TRAP LASER", true)
toggles["SALVAR BASE [X]"]={callback=function(v) if v then salvarPos() end end}
createToggle("SALVAR BASE [X]", true)
toggles["USAR TP [Q]"]={callback=function(v) if v then usarPos() end end}
createToggle("USAR TP [Q]", true)
toggles["MELHOR PALHAÇO [P]"]={callback=function(v) if v then tpPalhaco() end end}
createToggle("MELHOR PALHAÇO [P]", true)
toggles["AUTO HOP [R] OFF"]={callback=function(v) autoHop=v if v then notify("🔍 HOP ON","Caçando",2) else notify("HOP OFF","",2) end end}
createToggle("AUTO HOP [R] OFF", false)
createFechar()

-- HOTKEYS
UIS.InputBegan:Connect(function(i,gp) if gp then return end if i.KeyCode==Enum.KeyCode.M then main.Visible=not main.Visible openBtn.Visible=not main.Visible elseif i.KeyCode==Enum.KeyCode.P then tpPalhaco() elseif i.KeyCode==Enum.KeyCode.X then salvarPos() elseif i.KeyCode==Enum.KeyCode.Q then usarPos() elseif i.KeyCode==Enum.KeyCode.R then autoHop=not autoHop notify("R",autoHop and "HOP ON" or "HOP OFF",2) end end)

-- LOOPS
RS.Stepped:Connect(function() pcall(function() local char=LP.Character if not char then return end if noclip then for _,v in pairs(char:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide=false end end end if speedOn then local hum=char:FindFirstChildOfClass("Humanoid") if hum and hum.WalkSpeed~=150 then hum.WalkSpeed=150 end end if antiHit then local hum=char:FindFirstChildOfClass("Humanoid") if hum then hum:SetStateEnabled(Enum.HumanoidStateType.Ragdoll,false) hum:SetStateEnabled(Enum.HumanoidStateType.FallingDown,false) end end end) end)

task.spawn(function() task.wait(3) local bp=getBase() if bp then savedCFrame=CFrame.new(bp) end pcall(function() LP.Character.Humanoid.WalkSpeed=150 end) main.Visible=true end)

task.spawn(function() while true do task.wait(0.15) pcall(function() if espOn then for _,plot in pairs(Plots:GetChildren()) do if plot:FindFirstChild("Owner") and plot.Owner.Value~=LP.Name then for _,m in pairs(plot:GetChildren()) do if m:IsA("Model") and m.PrimaryPart and m:FindFirstChild("PricePerSecond") then local nome=string.lower(m.Name) if string.find(nome,"clown") or string.find(nome,"palhaco") or string.find(nome,"jester") or string.find(nome,"palha") then if not m:FindFirstChild("MIIIGUEX_HL") then criarESP(m,m.PricePerSecond.Value,plot.Owner.Value) end end end end end end end local char=LP.Character if not char then return end local hrp=char:FindFirstChild("HumanoidRootPart") if not hrp then return end local tool=char:FindFirstChildWhichIsA("Tool") if tool then local n=string.lower(tool.Name) if string.find(n,"cloak") or string.find(n,"invis") or string.find(n,"ghost") or string.find(n,"cape") then return end if tpAuto and savedCFrame then hrp.CFrame=savedCFrame task.wait(0.25) if autoInvis then for _,t in pairs(LP.Backpack:GetChildren()) do local nn=string.lower(t.Name) if string.find(nn,"cloak") or string.find(nn,"invis") or string.find(nn,"ghost") then LP.Character.Humanoid:EquipTool(t) task.wait(0.1) if t:FindFirstChild("Activate") then t:Activate() end end end end task.wait(0.1) local tt=char:FindFirstChildWhichIsA("Tool") if tt then local nnn=string.lower(tt.Name) if not (string.find(nnn,"cloak") or string.find(nnn,"invis") or string.find(nnn,"ghost") or string.find(nnn,"cape")) then tt.Parent=LP.Backpack end end end else local best,val,_=temMelhorPalhaco() if best then hrp.CFrame=best.PrimaryPart.CFrame+Vector3.new(0,0,2) task.wait(0.05) if autoClick then for i=1,5 do firetouchinterest(hrp,best.PrimaryPart,0) firetouchinterest(hrp,best.PrimaryPart,1) for _,d in pairs(best:GetDescendants()) do if d:IsA("ProximityPrompt") then fireproximityprompt(d) end end task.wait(0.05) end else firetouchinterest(hrp,best.PrimaryPart,0) firetouchinterest(hrp,best.PrimaryPart,1) for _,d in pairs(best:GetDescendants()) do if d:IsA("ProximityPrompt") then fireproximityprompt(d) end end end else if autoHop then ServerHop() task.wait(3) end end end end) end end)

notify("MIIIGUEX V28","Visual igual foto carregado! [M] abre/fecha",5)
