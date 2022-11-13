# Roblox-arsenal-script

local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Arsenal script", "DarkTheme")

--Main
local Main = Window:NewTab("Main")

--Aimbot

local MainSection = Main:NewSection("Aimbot")
MainSection:NewButton("Aimbot", "If u dont know what this is ur a retard", function()
    local teamCheck = true
local fov = 150
local smoothing = 1

local RunService = game:GetService("RunService")

local FOVring = Drawing.new("Circle")
FOVring.Visible = true
FOVring.Thickness = 1.5
FOVring.Radius = fov
FOVring.Transparency = 1
FOVring.Color = Color3.fromRGB(255, 128, 128)
FOVring.Position = workspace.CurrentCamera.ViewportSize/2

local function getClosest(cframe)
   local ray = Ray.new(cframe.Position, cframe.LookVector).Unit
   
   local target = nil
   local mag = math.huge
   
   for i,v in pairs(game.Players:GetPlayers()) do
       if v.Character and v.Character:FindFirstChild("Head") and v.Character:FindFirstChild("Humanoid") and v.Character:FindFirstChild("HumanoidRootPart") and v ~= game.Players.LocalPlayer and (v.Team ~= game.Players.LocalPlayer.Team or (not teamCheck)) then
           local magBuf = (v.Character.Head.Position - ray:ClosestPoint(v.Character.Head.Position)).Magnitude
           
           if magBuf < mag then
               mag = magBuf
               target = v
           end
       end
   end
   
   return target
end

loop = RunService.RenderStepped:Connect(function()
   local UserInputService = game:GetService("UserInputService")
   local pressed = --[[UserInputService:IsKeyDown(Enum.KeyCode.E)]] UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton2) --Enum.UserInputType.MouseButton2
   local localPlay = game.Players.localPlayer.Character
   local cam = workspace.CurrentCamera
   local zz = workspace.CurrentCamera.ViewportSize/2
   
   if pressed then
       local Line = Drawing.new("Line")
       local curTar = getClosest(cam.CFrame)
       local ssHeadPoint = cam:WorldToScreenPoint(curTar.Character.Head.Position)
       ssHeadPoint = Vector2.new(ssHeadPoint.X, ssHeadPoint.Y)
       if (ssHeadPoint - zz).Magnitude < fov then
           workspace.CurrentCamera.CFrame = workspace.CurrentCamera.CFrame:Lerp(CFrame.new(cam.CFrame.Position, curTar.Character.Head.Position), smoothing)
       end
   end
   
   if UserInputService:IsKeyDown(Enum.KeyCode.Delete) then
       loop:Disconnect()
       FOVring:Remove()
   end
end)
end)


--Hitboxai

local MainSection = Main:NewSection("Hitbox expander")

MainSection:NewButton("Hitbox expander", "Makes enemys thicc", function()
    function getnames()
        for i, v in pairs(game:GetChildren()) do
            if v.ClassName == "Players" then
                return v.Name
            end
        end
    end
    local players = getnames()
    local LP = game:GetService("Players").LocalPlayer
    coroutine.resume(
        coroutine.create(
            function()
                while wait(1) do
                    coroutine.resume(
                        coroutine.create(
                            function()
                                for _, v in pairs(game:GetService("Players"):GetPlayers()) do
                                    if v.Name ~= LP.Name and v.Character.UpperTorso.Color ~= LP.Character.UpperTorso.Color then
                                        v.Character.LowerTorso.CanCollide = false
                                        v.Character.LowerTorso.Material = "Neon"
                                        v.Character.LowerTorso.Size = Vector3.new(10, 10, 10)
                                        v.Character.HumanoidRootPart.Size = Vector3.new(15, 15, 15)
                                    end
                                end
                            end
                        )
                    )
                end
            end
        )
    )
end)
    


--ESP

local MainSection = Main:NewSection("ESP")

MainSection:NewButton("ESP", "", function()
    loadstring(game:HttpGet('https://raw.githubusercontent.com/ic3w0lf22/Unnamed-ESP/master/UnnamedESP.lua',true))()
end)

--Gun mods

local MainSection = Main:NewSection("Gun mods")

MainSection:NewButton("Gun mods", "", function()
    local replicationstorage = game.ReplicatedStorage

    for i, v in pairs(replicationstorage.Weapons:GetDescendants()) do
       if v.Name == "Auto" then
           v.Value = true
       end
       if v.Name == "RecoilControl" then
           v.Value = 0
       end
       if v.Name == "MaxSpread" then
           v.Value = 0
       end
       if v.Name == "ReloadTime" then
          v.Value = 1
       end
       if v.Name == "FireRate" then
           v.Value = 0.05
       end
       if v.Name == "Crit" then
           v.Value = 20
       end
    end
end)

--Wallbang

MainSection:NewLabel("Wallbang")

MainSection:NewButton("Wallbang", "Makes it so u can shoot trough walls", function()
    getgenv().Wallbang = true

-- normal patched wallbang

local mt = getrawmetatable(game)
local namecallold = mt.__namecall
setreadonly(mt, false)
mt.__namecall = newcclosure(function(self, ...)
    local Args = {...}
    NamecallMethod = getnamecallmethod()
    if getgenv().Wallbang and tostring(NamecallMethod) == "FindPartOnRayWithIgnoreList" then
        table.insert(Args[2], workspace.Map)
    end
    return namecallold(self, ...)
end)
-- WALLBANG BYPASS
loadstring(game:HttpGet("https://pastebin.pl/view/raw/93ee6b4f", true))() -- credits to bolts and the 3 bakers and Finny for this
setreadonly(mt, true)
end)


--Misc

local Misc = Window:NewTab("Misc")

--Ctrl + click tp

local MiscSection = Misc:NewSection("Ctrl + Click tp (Buggy)")
MiscSection:NewButton("Ctrl + Click tp", "Makes it so you can tp with ur left click while holding ctrl", function()
    local UIS = game:GetService("UserInputService")

local Player = game.Players.LocalPlayer
local Mouse = Player:GetMouse()


function GetCharacter()
   return game.Players.LocalPlayer.Character
end

function Teleport(pos)
   local Char = GetCharacter()
   if Char then
       Char:MoveTo(pos)
   end
end


UIS.InputBegan:Connect(function(input)
   if input.UserInputType == Enum.UserInputType.MouseButton1 and UIS:IsKeyDown(Enum.KeyCode.LeftControl) then
       Teleport(Mouse.Hit.p)
   end
end)
end)

--Speed

local MiscSection = Misc:NewSection("Speed")
MiscSection:NewToggle("Speed", "Makes u go zoom", function(state)
    if state then
        getgenv().WalkSpeedValue = 100; --set your desired walkspeed here
local Player = game:service'Players'.LocalPlayer;
Player.Character.Humanoid:GetPropertyChangedSignal'WalkSpeed':Connect(function()
Player.Character.Humanoid.WalkSpeed = getgenv().WalkSpeedValue;
end)
Player.Character.Humanoid.WalkSpeed = getgenv().WalkSpeedValue;
    else
        getgenv().WalkSpeedValue = 16; --set your desired walkspeed here
local Player = game:service'Players'.LocalPlayer;
Player.Character.Humanoid:GetPropertyChangedSignal'WalkSpeed':Connect(function()
Player.Character.Humanoid.WalkSpeed = getgenv().WalkSpeedValue;
end)
Player.Character.Humanoid.WalkSpeed = getgenv().WalkSpeedValue;
    end
end)

--Inf jump

local MiscSection = Misc:NewSection("Inf jump")

MiscSection:NewButton("Inf jump", "Makes it so u can jump alot", function()
    local InfiniteJumpEnabled = true
game:GetService("UserInputService").JumpRequest:connect(function()
	if InfiniteJumpEnabled then
		game:GetService"Players".LocalPlayer.Character:FindFirstChildOfClass'Humanoid':ChangeState("Jumping")
	end
end)
end)


--Fly

local MiscSection = Misc:NewSection("Fly")

MiscSection:NewButton("Fly", "This makes you fly. (E to toggle)", function()
    game:GetService("StarterGui"):SetCore("SendNotification", {Title = "Fly - Executed", Text = "Script made by forge4t"})
     
    repeat wait() 
        until game.Players.LocalPlayer and game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:findFirstChild("HumanoidRootPart") and game.Players.LocalPlayer.Character:findFirstChild("Humanoid") 
    local mouse = game.Players.LocalPlayer:GetMouse() 
    repeat wait() until mouse
    local plr = game.Players.LocalPlayer 
    local torso = plr.Character.HumanoidRootPart 
    local flying = true
    local deb = true 
    local ctrl = {f = 0, b = 0, l = 0, r = 0} 
    local lastctrl = {f = 0, b = 0, l = 0, r = 0} 
    local maxspeed = 30 
    local speed = 0 
     
    function Fly() 
    local bg = Instance.new("BodyGyro", torso) 
    bg.P = 9e4 
    bg.maxTorque = Vector3.new(9e9, 9e9, 9e9) 
    bg.cframe = torso.CFrame 
    local bv = Instance.new("BodyVelocity", torso) 
    bv.velocity = Vector3.new(0,0.1,0) 
    bv.maxForce = Vector3.new(9e9, 9e9, 9e9) 
    repeat wait() 
    plr.Character.Humanoid.PlatformStand = true 
    if ctrl.l + ctrl.r ~= 0 or ctrl.f + ctrl.b ~= 0 then 
    speed = speed+.5+(speed/maxspeed) 
    if speed > maxspeed then 
    speed = maxspeed 
    end 
    elseif not (ctrl.l + ctrl.r ~= 0 or ctrl.f + ctrl.b ~= 0) and speed ~= 0 then 
    speed = speed-1 
    if speed < 0 then 
    speed = 0 
    end 
    end 
    if (ctrl.l + ctrl.r) ~= 0 or (ctrl.f + ctrl.b) ~= 0 then 
    bv.velocity = ((game.Workspace.CurrentCamera.CoordinateFrame.lookVector * (ctrl.f+ctrl.b)) + ((game.Workspace.CurrentCamera.CoordinateFrame * CFrame.new(ctrl.l+ctrl.r,(ctrl.f+ctrl.b)*.2,0).p) - game.Workspace.CurrentCamera.CoordinateFrame.p))*speed 
    lastctrl = {f = ctrl.f, b = ctrl.b, l = ctrl.l, r = ctrl.r} 
    elseif (ctrl.l + ctrl.r) == 0 and (ctrl.f + ctrl.b) == 0 and speed ~= 0 then 
    bv.velocity = ((game.Workspace.CurrentCamera.CoordinateFrame.lookVector * (lastctrl.f+lastctrl.b)) + ((game.Workspace.CurrentCamera.CoordinateFrame * CFrame.new(lastctrl.l+lastctrl.r,(lastctrl.f+lastctrl.b)*.2,0).p) - game.Workspace.CurrentCamera.CoordinateFrame.p))*speed 
    else 
    bv.velocity = Vector3.new(0,0.1,0) 
    end 
    bg.cframe = game.Workspace.CurrentCamera.CoordinateFrame * CFrame.Angles(-math.rad((ctrl.f+ctrl.b)*50*speed/maxspeed),0,0) 
    until not flying 
    ctrl = {f = 0, b = 0, l = 0, r = 0} 
    lastctrl = {f = 0, b = 0, l = 0, r = 0} 
    speed = 0 
    bg:Destroy() 
    bv:Destroy() 
    plr.Character.Humanoid.PlatformStand = false 
    end 
    mouse.KeyDown:connect(function(key) 
    if key:lower() == "e" then 
    if flying then flying = false 
    else 
    flying = true 
    Fly() 
    end 
    elseif key:lower() == "w" then 
    ctrl.f = 1 
    elseif key:lower() == "s" then 
    ctrl.b = -1 
    elseif key:lower() == "a" then 
    ctrl.l = -1 
    elseif key:lower() == "d" then 
    ctrl.r = 1 
    end 
    end) 
    mouse.KeyUp:connect(function(key) 
    if key:lower() == "w" then 
    ctrl.f = 0 
    elseif key:lower() == "s" then 
    ctrl.b = 0 
    elseif key:lower() == "a" then 
    ctrl.l = 0 
    elseif key:lower() == "d" then 
    ctrl.r = 0 
    end 
    end)
    Fly()
    end)

--Info

local Info = Window:NewTab("Info")
local Section = Info:NewSection("Made by NoL1fe#6231")
local Section = Info:NewSection("Arsenals anticheat is lowkey shit")
