-- MIIIGUEX V29 - MINI + SCROLL + CANTO
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

local gui = Instance.new("ScreenGui", LP.PlayerGui) gui.Name = "MIIIGUEX_HUB" gui.ResetOnSpawn = false

-- MAIN MINI - NO CANTO
local main = Instance.new("Frame", gui) main.Size = UDim2.new(0,320,0,420) main.Position = UDim2.new(1,-335,1,-435) main.BackgroundColor3 = Color3.fromRGB(10,10,12) main.Active=true main.Draggable=true
Instance.new("UICorner", main).CornerRadius = UDim.new(0,16)
local strokeMain = Instance.new("UIStroke", main) strokeMain.Color = Color3.fromRGB(255,30,30) strokeMain.Thickness = 1.5

-- HEADER MINI
local header = Instance.new("Frame", main) header.Size = UDim2.new(1,-4,0,45) header.Position = UDim2.new(0,2,0,2) header.BackgroundColor3 = Color3.fromRGB(20,10,10) Instance.new("UICorner", header).CornerRadius = UDim.new(0,14)
local title = Instance.new("TextLabel", header) title.Size = UDim2.new(1,-90,1,0) title.Position = UDim2.new(0,45,0,0) title.Text="MIIIGUEX V29" title.TextColor3=Color3.fromRGB(255,50,50) title.Font=Enum.Font.GothamBlack title.TextSize=18 title.BackgroundTransparency=1 title.TextXAlignment=Enum.TextXAlignment.Left
local skull = Instance.new("TextLabel", header) skull.Size=UDim2.new(0,35,0,35) skull.Position=UDim2.new(0,5,0,5) skull.Text="💀" skull.TextSize=22 skull.BackgroundTransparency=1

-- BOTOES MINIMIZAR
local minBtn = Instance.new("TextButton", header) minBtn.Size=UDim2.new(0,30,0,30) minBtn.Position=UDim2.new(1,-65,0,7) minBtn.Text="-" minBtn.Font=Enum.Font.GothamBlack minBtn.TextSize=18 minBtn.BackgroundColor3=Color3.fromRGB(40,40,40) minBtn.TextColor3=Color3.new(1,1,1) Instance.new("UICorner", minBtn).CornerRadius=UDim.new(0,8)
local closeBtn = Instance.new("TextButton", header) closeBtn.Size=UDim2.new(0,30,0,30) closeBtn.Position=UDim2.new(1,-32,0,7) closeBtn.Text="X" closeBtn.Font=Enum.Font.GothamBold closeBtn.TextSize=14 closeBtn.BackgroundColor3=Color3.fromRGB(120,20,20) closeBtn.TextColor3=Color3.new(1,1,1) Instance.new("UICorner", closeBtn).CornerRadius=UDim.new(0,8)

-- SCROLL FRAME - PRA DESCER
local scroll = Instance.new("ScrollingFrame", main) scroll.Size=UDim2.new(1,-10,1,-55) scroll.Position=UDim2.new(0,5,0,50) scroll.BackgroundColor3=Color3.fromRGB(15,15,17) scroll.BorderSizePixel=0 scroll.ScrollBarThickness=4 scroll.ScrollBarImageColor3=Color3.fromRGB(255,30,30) scroll.CanvasSize=UDim2.new(0,0,0,650)
Instance.new("UICorner", scroll).CornerRadius=UDim.new(0,12)
local layout = Instance.new("UIListLayout", scroll) layout.Padding=UDim.new(0,5) layout.SortOrder=Enum.SortOrder.LayoutOrder
local pad = Instance.new("UIPadding", scroll) pad.PaddingTop=UDim.new(0,6) pad.PaddingBottom=UDim.new(0,6) pad.PaddingLeft=UDim.new(0,6) pad.PaddingRight=UDim.new(0,6)

-- BOTAO M MINI NO CANTO
local openBtn = Instance.new("TextButton", gui) openBtn.Size=UDim2.new(0,50,0,50) openBtn.Position=UDim2.new(0,10,0.5,-25) openBtn.Text="M" openBtn.Visible=false openBtn.BackgroundColor3=Color3.fromRGB(20,10,10) openBtn.TextColor3=Color3.fromRGB(255,50,50) openBtn.Font=Enum.Font.GothamBlack openBtn.TextSize=22 Instance.new("UICorner", openBtn).CornerRadius=UDim.new(0,25) local st=Instance.new("UIStroke", openBtn) st.Color=Color3.fromRGB(255,30,30) st.Thickness=2

-- FUNCAO TOGGLE MINI
local function createToggle(nome, ligado, parent)
    parent = parent or scroll
    local frame = Instance.new("Frame", parent) frame.Size=UDim2.new(1,-10,0,36) frame.BackgroundColor3=Color3.fromRGB(25,15,15) frame.LayoutOrder=1 Instance.new("UICorner", frame).CornerRadius=UDim.new(0,18)
    local s = Instance.new("UIStroke", frame) s.Color=Color3.fromRGB(60,20,20) s.Thickness=1
    local label = Instance.new("TextLabel", frame) label.Size=UDim2.new(1,-75,1,0) label.Position=UDim2.new(0,12,0,0) label.Text=nome label.TextColor3=Color3.new(1,1,1) label.Font=Enum.Font.GothamBold label.TextSize=11 label.BackgroundTransparency=1 label.TextXAlignment=Enum.TextXAlignment.Left

    local toggleBG = Instance.new("Frame", frame) toggleBG.Size=UDim2.new(0,50,0,24) toggleBG.Position=UDim2.new(1,-58,0.5,-12) toggleBG.BackgroundColor3=ligado and Color3.fromRGB(50,180,70) or Color3.fromRGB(70,70,70) Instance.new("UICorner", toggleBG).CornerRadius=UDim.new(1,0)
    local circle = Instance.new("Frame", toggleBG) circle.Size=UDim2.new(0,20,0,20) circle.Position=ligado and UDim2.new(1,-22,0.5,-10) or UDim2.new(0,2,0.5,-10) circle.BackgroundColor3=Color3.new(1,1,1) Instance.new("UICorner", circle).CornerRadius=UDim.new(1,0)

    local btn = Instance.new("TextButton", frame) btn.Size=UDim2.new(1,0,1,0) btn.BackgroundTransparency=1 btn.Text=""
    local state = ligado
    btn.MouseButton1Click:Connect(function()
        state = not state
        game:GetService("TweenService"):Create(toggleBG, TweenInfo.new(0.2), {BackgroundColor3=state and Color3.fromRGB(50,180,70) or Color3.fromRGB(70,70,70)}):Play()
        game:GetService("TweenService"):Create(circle, TweenInfo.new(0.2), {Position=state and UDim2.new(1,-22,0.5,-10) or UDim2.new(0,2,0.5,-10)}):Play()
        if _G.MIIIGUEX_TOGGLES and _G.MIIIGUEX_TOGGLES[nome] then _G.MIIIGUEX_TOGGLES[nome](state) end
    end)
    return frame
end

local function createBotaoVermelho(txt)
    local frame = Instance.new("Frame", scroll) frame.Size=UDim2.new(1,-10,0,38) frame.BackgroundColor3=Color3.fromRGB(160,20,20) Instance.new("UICorner", frame).CornerRadius=UDim.new(0,10)
    local label = Instance.new("TextLabel", frame) label.Size=UDim2.new(1,0,1,0) label.Text=txt label.TextColor3=Color3.new(1,1,1) label.Font=Enum.Font.GothamBlack label.TextSize=13 label.BackgroundTransparency=1
    local btn = Instance.new("TextButton", frame) btn.Size=UDim2.new(1,0,1,0) btn.BackgroundTransparency=1 btn.Text=""
    return btn
end

-- VARIAVEIS GLOBAIS
_G.MIIIGUEX_TOGGLES = {}
local noclip=true local autoK=true local tpAuto=true local speedOn=true local autoHop=false
local autoInvis=true local antiHit=true local autoClick=true local espOn=true local autoTrap=true
local savedCFrame=nil

local function getBase() for _,p in pairs(Plots:GetChildren()) do if p:FindFirstChild("Owner") and p.Owner.Value==LP.Name then return p:GetPivot().Position+Vector3.new(0,5,0) end end end
local function salvarPos() local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") if hrp then savedCFrame=hrp.CFrame notify("💾","Base salva!",2) end end
local function usarPos() if savedCFrame then local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") if hrp then hrp.CFrame=savedCFrame end end end
local function temMelhorPalhaco()
    local melhor, valor, dono = nil, 0, ""
    for _,plot in pairs(Plots:GetChildren()) do if plot:FindFirstChild("Owner") and plot.Owner.Value~=LP.Name then for _,m in pairs(plot:GetChildren()) do if m:IsA("Model") and m.PrimaryPart and m:FindFirstChild("PricePerSecond") then local nome=string.lower(m.Name) if string.find(nome,"clown") or string.find(nome,"palhaco") or string.find(nome,"jester") or string.find(nome,"palha") or string.find(nome,"circus") then if m.PricePerSecond.Value>valor then valor=m.PricePerSecond.Value melhor=m dono=plot.Owner.Value end end end end end end
    return melhor, valor, dono
end
local function tpPalhaco() local best,val,dono=temMelhorPalhaco() if best then local hrp=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") if hrp then hrp.CFrame=best.PrimaryPart.CFrame+Vector3.new(0,0,3) notify("🤡","$"..val.." | "..dono,2) end else notify("🤡","Sem palhaço",2) end end
local function ServerHop() notify("🔄","Hopping",2) table.insert(getgenv().Visitados, game.JobId) local PlaceId=game.PlaceId local servers={} local ok,res=pcall(function() return HS:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..PlaceId.."/servers/Public?sortOrder=Asc&limit=100")) end) if ok and res and res.data then for _,s in pairs(res.data) do if s.id~=game.JobId and s.playing<s.maxPlayers then local ja=false for _,v in pairs(getgenv().Visitados) do if v==s.id then ja=true break end end if not ja then table.insert(servers,s.id) end end end if #servers>0 then pcall(function() TS:TeleportToPlaceInstance(PlaceId, servers[math.random(1,#servers)], LP) end) task.wait(0.5) end end if #getgenv().Visitados>35 then getgenv().Visitados={} end pcall(function() TS:Teleport(PlaceId, LP) end) end

-- CRIAR TOGGLES
_G.MIIIGUEX_TOGGLES["SPEED 150"]=function(v) speedOn=v end createToggle("SPEED 150", true)
_G.MIIIGUEX_TOGGLES["NoClip"]=function(v) noclip=v end createToggle("NoClip", true)
_G.MIIIGUEX_TOGGLES["Auto Kitar [E]"]=function(v) autoK=v end createToggle("Auto Kitar [E]", true)
_G.MIIIGUEX_TOGGLES["Q AUTO"]=function(v) tpAuto=v end createToggle("Q AUTO AO PEGAR", true)
_G.MIIIGUEX_TOGGLES["AUTO INVIS"]=function(v) autoInvis=v end createToggle("AUTO INVIS AO PEGAR", true)
_G.MIIIGUEX_TOGGLES["ANTI-HIT"]=function(v) antiHit=v end createToggle("ANTI-HIT TASER", true)
_G.MIIIGUEX_TOGGLES["AUTO CLICK"]=function(v) autoClick=v end createToggle("AUTO CLICK TURBO", true)
_G.MIIIGUEX_TOGGLES["ESP"]=function(v) espOn=v if not v then for _,p in pairs(Plots:GetChildren()) do for _,m in pairs(p:GetChildren()) do if m:FindFirstChild("MIIIGUEX_HL") then m.MIIIGUEX_HL:Destroy() end if m.PrimaryPart and m.PrimaryPart:FindFirstChild("MIIIGUEX_ESP") then m.PrimaryPart.MIIIGUEX_ESP:Destroy() end end end end end createToggle("ESP PALHAÇO", true)
_G.MIIIGUEX_TOGGLES["AUTO TRAP"]=function(v) autoTrap=v end createToggle("AUTO TRAP LASER", true)
_G.MIIIGUEX_TOGGLES["SALVAR [X]"]=function(v) if v then salvarPos() end end createToggle("SALVAR BASE [X]", true)
_G.MIIIGUEX_TOGGLES["USAR [Q]"]=function(v) if v then usarPos() end end createToggle("USAR TP [Q]", true)
_G.MIIIGUEX_TOGGLES["MELHOR [P]"]=function(v) if v then tpPalhaco() end end createToggle("MELHOR PALHAÇO [P]", true)
_G.MIIIGUEX_TOGGLES["HOP [R]"]=function(v) autoHop=v end createToggle("AUTO HOP [R]", false)

local btnFechar = createBotaoVermelho("FECHAR [M]")
btnFechar.MouseButton1Click:Connect(function() main.Visible=false openBtn.Visible=true end)

-- MINIMIZAR
local minimizado=false
minBtn.MouseButton1Click:Connect(function()
    minimizado=not minimizado
    if minimizado then
        game:GetService("TweenService"):Create(main, TweenInfo.new(0.3), {Size=UDim2.new(0,320,0,50)}):Play()
        scroll.Visible=false minBtn.Text="+"
    else
        game:GetService("TweenService"):Create(main, TweenInfo.new(0.3), {Size=UDim2.new(0,320,0,420)}):Play()
        scroll.Visible=true minBtn.Text="-"
    end
end)
closeBtn.MouseButton1Click:Connect(function() main.Visible=false openBtn.Visible=true end)
openBtn.MouseButton1Click:Connect(function() main.Visible=true openBtn.Visible=false end)

UIS.InputBegan:Connect(function(i,gp) if gp then return end if i.KeyCode==Enum.KeyCode.M then main.Visible=not main.Visible openBtn.Visible=not main.Visible elseif i.KeyCode==Enum.KeyCode.P then tpPalhaco() elseif i.KeyCode==Enum.KeyCode.X then salvarPos() elseif i.KeyCode==Enum.KeyCode.Q then usarPos() elseif i.KeyCode==Enum.KeyCode.R then autoHop=not autoHop notify("R",autoHop and "HOP ON" or "OFF",2) end end)

RS.Stepped:Connect(function() pcall(function() local char=LP.Character if not char then return end if noclip then for _,v in pairs(char:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide=false end end end if speedOn then local hum=char:FindFirstChildOfClass("Humanoid") if hum and hum.WalkSpeed~=150 then hum.WalkSpeed=150 end end if antiHit then local hum=char:FindFirstChildOfClass("Humanoid") if hum then hum:SetStateEnabled(Enum.HumanoidStateType.Ragdoll,false) hum:SetStateEnabled(Enum.HumanoidStateType.FallingDown,false) end end end) end)

task.spawn(function() task.wait(2) local bp=getBase() if bp then savedCFrame=CFrame.new(bp) end pcall(function() LP.Character.Humanoid.WalkSpeed=150 end) end)

task.spawn(function() while true do task.wait(0.15) pcall(function() if espOn then for _,plot in pairs(Plots:GetChildren()) do if plot:FindFirstChild("Owner") and plot.Owner.Value~=LP.Name then for _,m in pairs(plot:GetChildren()) do if m:IsA("Model") and m.PrimaryPart and m:FindFirstChild("PricePerSecond") then local nome=string.lower(m.Name) if string.find(nome,"clown") or string.find(nome,"palhaco") or string.find(nome,"jester") then if not m:FindFirstChild("MIIIGUEX_HL") then local bb=Instance.new("BillboardGui", m.PrimaryPart) bb.Name="MIIIGUEX_ESP" bb.Size=UDim2.new(0,200,0,40) bb.StudsOffset=Vector3.new(0,4,0) bb.AlwaysOnTop=true local txt=Instance.new("TextLabel", bb) txt.Size=UDim2.new(1,0,1,0) txt.BackgroundTransparency=1 txt.Text="🤡 $"..m.PricePerSecond.Value txt.TextColor3=Color3.fromRGB(255,255,0) txt.Font=Enum.Font.GothamBold txt.TextSize=13 local hl=Instance.new("Highlight", m) hl.Name="MIIIGUEX_HL" hl.FillColor=Color3.fromRGB(255,0,0) hl.OutlineColor=Color3.fromRGB(255,255,0) hl.FillTransparency=0.6 end end end end end end end local char=LP.Character if not char then return end local hrp=char:FindFirstChild("HumanoidRootPart") if not hrp then return end local tool=char:FindFirstChildWhichIsA("Tool") if tool then local n=string.lower(tool.Name) if string.find(n,"cloak") or string.find(n,"invis") or string.find(n,"ghost") or string.find(n,"cape") then return end if tpAuto and savedCFrame then hrp.CFrame=savedCFrame task.wait(0.25) if autoInvis then for _,t in pairs(LP.Backpack:GetChildren()) do local nn=string.lower(t.Name) if string.find(nn,"cloak") or string.find(nn,"invis") or string.find(nn,"ghost") then LP.Character.Humanoid:EquipTool(t) task.wait(0.1) if t:FindFirstChild("Activate") then t:Activate() end end end end task.wait(0.1) local tt=char:FindFirstChildWhichIsA("Tool") if tt then local nnn=string.lower(tt.Name) if not (string.find(nnn,"cloak") or string.find(nnn,"invis") or string.find(nnn,"ghost") or string.find(nnn,"cape")) then tt.Parent=LP.Backpack end end end else local best,val,_=temMelhorPalhaco() if best then hrp.CFrame=best.PrimaryPart.CFrame+Vector3.new(0,0,2) task.wait(0.05) if autoClick then for i=1,5 do firetouchinterest(hrp,best.PrimaryPart,0) firetouchinterest(hrp,best.PrimaryPart,1) for _,d in pairs(best:GetDescendants()) do if d:IsA("ProximityPrompt") then fireproximityprompt(d) end end task.wait(0.05) end else firetouchinterest(hrp,best.PrimaryPart,0) firetouchinterest(hrp,best.PrimaryPart,1) for _,d in pairs(best:GetDescendants()) do if d:IsA("ProximityPrompt") then fireproximityprompt(d) end end end else if autoHop then ServerHop() task.wait(3) end end end end) end end)

notify("V29 MINI","Arrasta pro canto + scroll pra descer | M minimiza",4)
