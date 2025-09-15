local Players=game:GetService("Players")
local LP=Players.LocalPlayer
local UIS=game:GetService("UserInputService")
local TS=game:GetService("TeleportService")
local HS=game:GetService("HttpService")

local function safeGuiParent()
    local ok,ui=pcall(function() if gethui then return gethui() end return game.CoreGui end)
    if ok and ui then return ui end
    return LP:WaitForChild("PlayerGui")
end

do
    local root=safeGuiParent()
    local old=root:FindFirstChild("SnuffyHubUI_Min")
    if old then old:Destroy() end
end

local PANEL_BG=Color3.fromRGB(15,15,15)
local PANEL_ALPHA=0.28
local STROKE_ALPHA=0.65
local TOL=10
local DEFAULT_VAL=10000000
local BASE_FOLDER_NAME="Events"

local PET_VALUES={
  ["Blackhole Goat"]=100000000,["Bisonte Giuppitere"]=90000000,["La Supreme Combinasion"]=85000000,
  ["Los Spyderinis"]=75000000,["Los Matteos"]=65000000,["Sammyni Spyderini"]=60000000,
  ["La Vacca Saturno Saturnita"]=55000000,["Torrtuginni Dragonfrutini"]=52000000,["Los Tralaleritos"]=48000000,
  ["Las Tralaleritas"]=45000000,["Las Vaquitas Saturnitas"]=40000000,["Graipuss Medussi"]=35000000,
  ["Pot Hotspot"]=32000000,["La Grande Combinasion"]=30000000,["Nuclearo Dinossauro"]=28000000,
  ["Garama and Madundung"]=26000000,["Chicleteira Bicicleteira"]=24000000,["Secret Lucky Block"]=22000000,
  ["Los Combinasionas"]=20000000,["Dragon Cannelloni"]=18000000,["Dul Dul Dul"]=16000000,
  ["Karkerkar Kurkur"]=14000000,["Los Hotspotsitos"]=12000000,["Esok Sekolah"]=11000000,
  ["Ketupat Kepat"]=10000000,["Job Job Job Sahur"]=9000000,["Noo My Hotspot"]=8000000,
  ["Strawberry Elephant"]=7000000,["Spaghetti Tualetti"]=6000000,["Guerriro Digitale"]=5000000,
  ["Ketchuru and Musturu"]=4000000,
}

local function baseFolder()
    if BASE_FOLDER_NAME~="" then
        local f=workspace:FindFirstChild(BASE_FOLDER_NAME)
        if f then return f end
    end
    return workspace
end

local function bestPetValueInServer()
    local root=baseFolder()
    local best=0
    for _,obj in ipairs(root:GetDescendants()) do
        local v=PET_VALUES[obj.Name]
        if v and v>best then best=v end
    end
    return best
end

local function withinTol(v,t,p) return math.abs(v-t)<=t*(p/100) end

local ScreenGui=Instance.new("ScreenGui")
ScreenGui.Name="SnuffyHubUI_Min"
ScreenGui.ResetOnSpawn=false
ScreenGui.Parent=safeGuiParent()

local Main=Instance.new("Frame")
Main.Name="MainPanel"
Main.Size=UDim2.new(0,340,0,220)
Main.Position=UDim2.new(0.5,-170,0.5,-110)
Main.BackgroundColor3=PANEL_BG
Main.BackgroundTransparency=PANEL_ALPHA
Main.Active=true
Main.Parent=ScreenGui
Instance.new("UICorner",Main).CornerRadius=UDim.new(0,12)
local MainStroke=Instance.new("UIStroke",Main)
MainStroke.Thickness=1.5
MainStroke.Color=Color3.new(1,1,1)
MainStroke.Transparency=STROKE_ALPHA

local Header=Instance.new("Frame")
Header.Size=UDim2.new(1,-20,0,32)
Header.Position=UDim2.new(0,10,0,10)
Header.BackgroundColor3=PANEL_BG
Header.BackgroundTransparency=PANEL_ALPHA
Header.Active=true
Header.Parent=Main
Instance.new("UICorner",Header).CornerRadius=UDim.new(0,8)

local Title=Instance.new("TextLabel")
Title.Size=UDim2.new(1,-90,1,0)
Title.Position=UDim2.new(0,10,0,0)
Title.BackgroundTransparency=1
Title.Text="Snuffy Hub — AUTO MATCHER"
Title.Font=Enum.Font.GothamSemibold
Title.TextSize=16
Title.TextColor3=Color3.fromRGB(235,235,235)
Title.TextXAlignment=Enum.TextXAlignment.Left
Title.Parent=Header

local function makeBtn(parent,txt,size,pos)
    local b=Instance.new("TextButton")
    b.Size=size;b.Position=pos
    b.Text=txt;b.Font=Enum.Font.GothamBold;b.TextSize=16
    b.BackgroundColor3=PANEL_BG;b.BackgroundTransparency=PANEL_ALPHA
    b.TextColor3=Color3.fromRGB(255,255,255)
    b.AutoButtonColor=true
    Instance.new("UICorner",b).CornerRadius=UDim.new(0,6)
    b.Parent=parent
    return b
end

local CloseBtn=makeBtn(Header,"X",UDim2.new(0,26,0,22),UDim2.new(1,-34,0.5,-9))
local MinBtn=makeBtn(Header,"▁",UDim2.new(0,26,0,22),UDim2.new(1,-66,0.5,-9))

local InputWrapper=Instance.new("Frame")
InputWrapper.Size=UDim2.new(1,-40,0,36)
InputWrapper.Position=UDim2.new(0,20,0,58)
InputWrapper.BackgroundTransparency=1
InputWrapper.Parent=Main
Instance.new("UICorner",InputWrapper).CornerRadius=UDim.new(0,10)
local IWStroke=Instance.new("UIStroke",InputWrapper)
IWStroke.Thickness=1.8
IWStroke.Color=Color3.fromRGB(210,210,210)
IWStroke.Transparency=0.45
IWStroke.ApplyStrokeMode=Enum.ApplyStrokeMode.Border

local TargetBox=Instance.new("TextBox")
TargetBox.Size=UDim2.new(1,-16,1,-8)
TargetBox.Position=UDim2.new(0,8,0,4)
TargetBox.Text=""
TargetBox.PlaceholderText="Digite o valor (ex.: 10M)"
TargetBox.Font=Enum.Font.GothamBold
TargetBox.TextSize=16
TargetBox.BackgroundTransparency=1
TargetBox.TextXAlignment=Enum.TextXAlignment.Left
TargetBox.TextColor3=Color3.fromRGB(255,255,255)
TargetBox.TextStrokeColor3=Color3.fromRGB(0,0,0)
TargetBox.TextStrokeTransparency=0.85
TargetBox.ClearTextOnFocus=false
TargetBox.Parent=InputWrapper

TargetBox:GetPropertyChangedSignal("Text"):Connect(function()
    local txt=TargetBox.Text or ""
    local digits=txt:gsub("%D","")
    if digits~=txt then TargetBox.Text=digits TargetBox.CursorPosition=#digits+1 end
end)
TargetBox.Focused:Connect(function() IWStroke.Transparency=0.05 IWStroke.Thickness=2.2 end)
TargetBox.FocusLost:Connect(function() IWStroke.Transparency=0.45 IWStroke.Thickness=1.8 end)

local AutoBtn=Instance.new("TextButton")
AutoBtn.Size=UDim2.new(1,-40,0,40)
AutoBtn.Position=UDim2.new(0,20,0,104)
AutoBtn.Text="AUTO MATCHER"
AutoBtn.Font=Enum.Font.GothamBold
AutoBtn.TextSize=18
AutoBtn.BackgroundColor3=Color3.fromRGB(200,0,0)
AutoBtn.TextColor3=Color3.fromRGB(255,255,255)
AutoBtn.Parent=Main
Instance.new("UICorner",AutoBtn).CornerRadius=UDim.new(0,8)

local TikTok=Instance.new("TextLabel")
TikTok.Size=UDim2.new(1,-40,0,20)
TikTok.Position=UDim2.new(0,20,0,154)
TikTok.BackgroundTransparency=1
TikTok.Text="@snuffy.hub (TikTok)"
TikTok.Font=Enum.Font.Gotham
TikTok.TextSize=14
TikTok.TextColor3=Color3.fromRGB(180,180,255)
TikTok.TextXAlignment=Enum.TextXAlignment.Left
TikTok.Parent=Main

local Discord=Instance.new("TextLabel")
Discord.Size=UDim2.new(1,-40,0,20)
Discord.Position=UDim2.new(0,20,0,176)
Discord.BackgroundTransparency=1
Discord.Text="discord.gg/RkEcPQEd2G"
Discord.Font=Enum.Font.Gotham
Discord.TextSize=14
Discord.TextColor3=Color3.fromRGB(150,255,150)
Discord.TextXAlignment=Enum.TextXAlignment.Left
Discord.Parent=Main

local MiniBar=Instance.new("Frame")
MiniBar.Name="SnuffyMiniBar"
MiniBar.Size=UDim2.new(0,260,0,26)
MiniBar.Position=UDim2.new(0.5,-130,0,20)
MiniBar.Visible=false
MiniBar.BackgroundColor3=PANEL_BG
MiniBar.BackgroundTransparency=PANEL_ALPHA
MiniBar.Active=true
MiniBar.Parent=ScreenGui
Instance.new("UICorner",MiniBar).CornerRadius=UDim.new(0,10)
local miniStroke=Instance.new("UIStroke",MiniBar)
miniStroke.Thickness=1.2;miniStroke.Color=Color3.fromRGB(255,255,255);miniStroke.Transparency=STROKE_ALPHA
local MiniTitle=Instance.new("TextLabel",MiniBar)
MiniTitle.Size=UDim2.new(1,-40,1,0)
MiniTitle.Position=UDim2.new(0,10,0,0)
MiniTitle.BackgroundTransparency=1
MiniTitle.Text="Snuffy Hub — Minimizado"
MiniTitle.Font=Enum.Font.GothamSemibold
MiniTitle.TextSize=14
MiniTitle.TextColor3=Color3.fromRGB(235,235,235)
MiniTitle.TextXAlignment=Enum.TextXAlignment.Left
local RestoreBtn=makeBtn(MiniBar,"▁",UDim2.new(0,24,0,20),UDim2.new(1,-32,0.5,-10))

do
    local dragging=false;local dragStart;local startPos
    local function begin(i) dragging=true;dragStart=i.Position;startPos=Main.Position
        i.Changed:Connect(function() if i.UserInputState==Enum.UserInputState.End then dragging=false end end) end
    local function update(i) if dragging then local d=i.Position-dragStart
        Main.Position=UDim2.new(startPos.X.Scale,startPos.X.Offset+d.X,startPos.Y.Scale,startPos.Y.Offset+d.Y) end end
    Header.InputBegan:Connect(function(i) if i.UserInputType==Enum.UserInputType.MouseButton1 or i.UserInputType==Enum.UserInputType.Touch then begin(i) end end)
    Header.InputChanged:Connect(function(i) if i.UserInputType==Enum.UserInputType.MouseMovement or i.UserInputType==Enum.UserInputType.Touch then update(i) end end)
    UIS.InputChanged:Connect(function(i) if i.UserInputType==Enum.UserInputType.MouseMovement or i.UserInputType==Enum.UserInputType.Touch then update(i) end end)
end

do
    local dragging=false;local dragStart;local startPos
    local function begin(i) dragging=true;dragStart=i.Position;startPos=MiniBar.Position
        i.Changed:Connect(function() if i.UserInputState==Enum.UserInputState.End then dragging=false end end) end
    local function update(i) if dragging then local d=i.Position-dragStart
        MiniBar.Position=UDim2.new(startPos.X.Scale,startPos.X.Offset+d.X,startPos.Y.Scale,startPos.Y.Offset+d.Y) end end
    MiniBar.InputBegan:Connect(function(i) if i.UserInputType==Enum.UserInputType.MouseButton1 or i.UserInputType==Enum.UserInputType.Touch then begin(i) end end)
    UIS.InputChanged:Connect(function(i) if i.UserInputType==Enum.UserInputType.MouseMovement or i.UserInputType==Enum.UserInputType.Touch then update(i) end end)
end

local lastMainPos=Main.Position
makeBtn(Header,"",UDim2.new(0,0,0,0),UDim2.new()) -- noop to keep makeBtn referenced
MinBtn.MouseButton1Click:Connect(function()
    lastMainPos=Main.Position
    Main.Visible=false
    MiniBar.Position=UDim2.new(0.5,-130,0,math.max(12,Main.AbsolutePosition.Y))
    MiniBar.Visible=true
end)
RestoreBtn.MouseButton1Click:Connect(function()
    MiniBar.Visible=false
    Main.Visible=true
    Main.Position=lastMainPos
end)
CloseBtn.MouseButton1Click:Connect(function()
    Main.Visible=false
    MiniBar.Visible=false
end)

local AUTO_ON=false
local _supervisorStarted=false
local blacklist={[game.JobId]=true}
local BLACKLIST_LIMIT=600
local function bl_add(id)
    blacklist[id]=true
    local c=0;for _ in pairs(blacklist) do c+=1 end
    if c>BLACKLIST_LIMIT then blacklist={[game.JobId]=true} end
end

local function fetchPage(cursor,asc)
    local url=("https://games.roblox.com/v1/games/%d/servers/Public?sortOrder=%s&limit=100%s")
        :format(game.PlaceId,asc and "Asc" or "Desc",cursor and ("&cursor="..cursor) or "")
    local ok,body=pcall(function() return game:HttpGet(url) end)
    if not ok then return nil end
    local ok2,data=pcall(function() return HS:JSONDecode(body) end)
    if not ok2 then return nil end
    return data
end

local function pickServerId()
    local asc=true
    local cursor=nil
    while AUTO_ON do
        local data=fetchPage(cursor,asc)
        if data then
            for _,srv in ipairs(data.data or {}) do
                if not blacklist[srv.id] and srv.playing<srv.maxPlayers then
                    return srv.id
                end
            end
            cursor=data.nextPageCursor
            if not cursor then asc=not asc cursor=nil end
        else
            task.wait(0.35)
        end
        task.wait(0.02)
    end
end

local function teleportTo(jobId)
    pcall(function() TS:TeleportToPlaceInstance(game.PlaceId,jobId,Players.LocalPlayer) end)
end

local function currentMatches(targetNum)
    local best=bestPetValueInServer()
    if best<=0 then return false,best end
    return withinTol(best,targetNum,TOL),best
end

local function startSupervisorOnce()
    if _supervisorStarted then return end
    _supervisorStarted=true
    task.spawn(function()
        while true do
            if AUTO_ON then
                local target=tonumber(TargetBox.Text)
                if not target or target<=0 then target=DEFAULT_VAL end
                local okMatch=select(1,currentMatches(target))
                if okMatch then
                    task.wait(0.8)
                else
                    local jobId=pickServerId()
                    if jobId then
                        bl_add(jobId)
                        teleportTo(jobId)
                        task.wait(0.8)
                    else
                        task.wait(0.35)
                    end
                end
            end
            task.wait(0.15)
        end
    end)
end
startSupervisorOnce()

AutoBtn.MouseButton1Click:Connect(function()
    AUTO_ON=not AUTO_ON
    AutoBtn.BackgroundColor3=AUTO_ON and Color3.fromRGB(0,170,0) or Color3.fromRGB(200,0,0)
end)
