--// Services
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")
local Camera = workspace.CurrentCamera
local Lighting = game:GetService("Lighting")
local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

--// GUI Setup
local ScreenGui = Instance.new("ScreenGui", CoreGui)
ScreenGui.Name = "MinHubV2"
ScreenGui.ResetOnSpawn = false

-- Function to create rounded corners
local function applyCornerRadius(instance, radius)
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, radius)
    corner.Parent = instance
end

local Main = Instance.new("Frame", ScreenGui)
Main.Size = UDim2.new(0, 450, 0, 350)
Main.Position = UDim2.new(0.5, -225, 0.5, -175)
Main.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
Main.BorderSizePixel = 0
Main.Active = true
Main.Draggable = true
applyCornerRadius(Main, 8)

-- Close Button
local CloseButton = Instance.new("TextButton", Main)
CloseButton.Size = UDim2.new(0, 30, 0, 20)
CloseButton.Position = UDim2.new(1, -35, 0, 5)
CloseButton.Text = "X"
CloseButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
CloseButton.TextColor3 = Color3.new(1, 1, 1)
applyCornerRadius(CloseButton, 4)

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

local Title = Instance.new("TextLabel", Main)
Title.Size = UDim2.new(1, -40, 0, 30)
Title.Position = UDim2.new(0, 5, 0, 0)
Title.Text = "Min Hub v2"
Title.TextColor3 = Color3.new(1, 1, 1)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 20
Title.BackgroundTransparency = 1
Title.TextXAlignment = Enum.TextXAlignment.Left

-- Rainbow Title
task.spawn(function()
    local hue = 0
    while true do
        hue = (hue + 0.01) % 1
        Title.TextColor3 = Color3.fromHSV(hue, 1, 1)
        task.wait(0.05)
    end
end)

-- Tabs
local Tabs = {}
local CurrentTab

local function createTab(name)
    local button = Instance.new("TextButton", Main)
    button.Size = UDim2.new(0, 100, 0, 30)
    button.Position = UDim2.new(#Tabs * 0.25, 0, 0, 30)
    button.Text = name
    button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    button.TextColor3 = Color3.new(1, 1, 1)
    button.Font = Enum.Font.Gotham
    button.TextSize = 14
    applyCornerRadius(button, 4)

    local tabFrame = Instance.new("Frame", Main)
    tabFrame.Size = UDim2.new(1, 0, 1, -60)
    tabFrame.Position = UDim2.new(0, 0, 0, 60)
    tabFrame.Visible = false
    tabFrame.BackgroundTransparency = 1

    button.MouseButton1Click:Connect(function()
        if CurrentTab then CurrentTab.Visible = false end
        tabFrame.Visible = true
        CurrentTab = tabFrame
    end)

    table.insert(Tabs, tabFrame)
    return tabFrame
end

local playerTab = createTab("Player")
local visualTab = createTab("Visual")
local trollTab = createTab("Troll")
local utilityTab = createTab("Utility")

-- Settings GUI Color
local settingsFrame = Instance.new("Frame", Main)
settingsFrame.Size = UDim2.new(0, 200, 0, 50)
settingsFrame.Position = UDim2.new(1, -200, 1, 0)
settingsFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
applyCornerRadius(settingsFrame, 8)

local colorPicker = Instance.new("TextBox", settingsFrame)
colorPicker.Size = UDim2.new(1, -10, 1, -10)
colorPicker.Position = UDim2.new(0, 5, 0, 5)
colorPicker.PlaceholderText = "Enter color hex (e.g. FF0000)"
colorPicker.Text = ""
colorPicker.TextColor3 = Color3.new(1, 1, 1)
colorPicker.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
colorPicker.ClearTextOnFocus = false
applyCornerRadius(colorPicker, 4)

colorPicker.FocusLost:Connect(function()
    local success, result = pcall(function()
        return Color3.fromHex("#" .. colorPicker.Text)
    end)
    if success then
        Main.BackgroundColor3 = result
    end
end)

--[[
    =====================================
            PLAYER TAB FEATURES
    =====================================
--]]

-- Click to Teleport
local teleporting = false
local clickTP = Instance.new("TextButton", playerTab)
clickTP.Size = UDim2.new(0, 200, 0, 30)
clickTP.Position = UDim2.new(0, 10, 0, 10)
clickTP.Text = "Click to Teleport: OFF"
clickTP.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
applyCornerRadius(clickTP, 4)

clickTP.MouseButton1Click:Connect(function()
    teleporting = not teleporting
    clickTP.Text = "Click to Teleport: " .. (teleporting and "ON" or "OFF")
end)

UIS.InputBegan:Connect(function(input)
    if teleporting and input.UserInputType == Enum.UserInputType.MouseButton1 then
        local mouse = LocalPlayer:GetMouse()
        if mouse then
            LocalPlayer.Character:MoveTo(mouse.Hit.Position)
        end
    end
end)

-- Sliders
local function createSlider(parent, name, min, max, posY, callback)
    local label = Instance.new("TextLabel", parent)
    label.Size = UDim2.new(0, 200, 0, 20)
    label.Position = UDim2.new(0, 10, 0, posY)
    label.Text = name
    label.BackgroundTransparency = 1
    label.TextColor3 = Color3.new(1, 1, 1)

    local box = Instance.new("TextBox", parent)
    box.Size = UDim2.new(0, 200, 0, 30)
    box.Position = UDim2.new(0, 10, 0, posY + 20)
    box.PlaceholderText = "Between " .. min .. " and " .. max
    box.Text = ""
    box.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    box.ClearTextOnFocus = false
    applyCornerRadius(box, 4)

    box.FocusLost:Connect(function()
        local val = tonumber(box.Text)
        if val then
            val = math.clamp(val, min, max)
            callback(val)
        end
    end)
end

createSlider(playerTab, "Speed", 16, 200, 50, function(val)
    LocalPlayer.Character.Humanoid.WalkSpeed = val
end)

createSlider(playerTab, "Jump Power", 50, 300, 120, function(val)
    LocalPlayer.Character.Humanoid.JumpPower = val
end)

-- Fly Exploit
local flying = false
local flySpeed = 50
local flyToggle = Instance.new("TextButton", playerTab)
flyToggle.Size = UDim2.new(0, 200, 0, 30)
flyToggle.Position = UDim2.new(0, 10, 0, 190)
flyToggle.Text = "Fly: OFF"
flyToggle.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
applyCornerRadius(flyToggle, 4)

local flyConnection
flyToggle.MouseButton1Click:Connect(function()
    flying = not flying
    flyToggle.Text = "Fly: " .. (flying and "ON" or "OFF")
    
    if flying then
        local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.PlatformStand = true
        end
        
        flyConnection = RunService.Stepped:Connect(function()
            if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                local root = LocalPlayer.Character.HumanoidRootPart
                local cam = workspace.CurrentCamera.CFrame
                
                local vel = Vector3.new()
                if UIS:IsKeyDown(Enum.KeyCode.W) then
                    vel = vel + (cam.LookVector * flySpeed)
                end
                if UIS:IsKeyDown(Enum.KeyCode.S) then
                    vel = vel - (cam.LookVector * flySpeed)
                end
                if UIS:IsKeyDown(Enum.KeyCode.A) then
                    vel = vel - (cam.RightVector * flySpeed)
                end
                if UIS:IsKeyDown(Enum.KeyCode.D) then
                    vel = vel + (cam.RightVector * flySpeed)
                end
                
                root.Velocity = vel
                root.RotVelocity = Vector3.new()
            end
        end)
    else
        if flyConnection then
            flyConnection:Disconnect()
            flyConnection = nil
        end
        local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.PlatformStand = false
        end
    end
end)

-- Infinite Jump
local infiniteJump = false
local infJumpToggle = Instance.new("TextButton", playerTab)
infJumpToggle.Size = UDim2.new(0, 200, 0, 30)
infJumpToggle.Position = UDim2.new(0, 10, 0, 230)
infJumpToggle.Text = "Infinite Jump: OFF"
infJumpToggle.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
applyCornerRadius(infJumpToggle, 4)

local jumpConnection
infJumpToggle.MouseButton1Click:Connect(function()
    infiniteJump = not infiniteJump
    infJumpToggle.Text = "Infinite Jump: " .. (infiniteJump and "ON" or "OFF")
    
    if infiniteJump then
        jumpConnection = UIS.JumpRequest:Connect(function()
            local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
            end
        end)
    else
        if jumpConnection then
            jumpConnection:Disconnect()
            jumpConnection = nil
        end
    end
end)

-- NoClip
local noclip = false
local noclipToggle = Instance.new("TextButton", playerTab)
noclipToggle.Size = UDim2.new(0, 200, 0, 30)
noclipToggle.Position = UDim2.new(0, 220, 0, 10)
noclipToggle.Text = "NoClip: OFF"
noclipToggle.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
applyCornerRadius(noclipToggle, 4)

local noclipConnection
noclipToggle.MouseButton1Click:Connect(function()
    noclip = not noclip
    noclipToggle.Text = "NoClip: " .. (noclip and "ON" or "OFF")
    
    if noclip then
        noclipConnection = RunService.Stepped:Connect(function()
            if LocalPlayer.Character then
                for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        end)
    else
        if noclipConnection then
            noclipConnection:Disconnect()
            noclipConnection = nil
        end
    end
end)

--[[
    =====================================
            VISUAL TAB FEATURES
    =====================================
--]]

-- FullBright
local fullbright = false
local fullbrightToggle = Instance.new("TextButton", visualTab)
fullbrightToggle.Size = UDim2.new(0, 200, 0, 30)
fullbrightToggle.Position = UDim2.new(0, 10, 0, 10)
fullbrightToggle.Text = "FullBright: OFF"
fullbrightToggle.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
applyCornerRadius(fullbrightToggle, 4)

local originalBrightness = Lighting.Brightness
local originalAmbient = Lighting.Ambient
local originalOutdoorAmbient = Lighting.OutdoorAmbient

fullbrightToggle.MouseButton1Click:Connect(function()
    fullbright = not fullbright
    fullbrightToggle.Text = "FullBright: " .. (fullbright and "ON" or "OFF")
    
    if fullbright then
        originalBrightness = Lighting.Brightness
        originalAmbient = Lighting.Ambient
        originalOutdoorAmbient = Lighting.OutdoorAmbient
        
        Lighting.Brightness = 2
        Lighting.Ambient = Color3.new(1, 1, 1)
        Lighting.OutdoorAmbient = Color3.new(1, 1, 1)
        Lighting.FogEnd = 100000
    else
        Lighting.Brightness = originalBrightness
        Lighting.Ambient = originalAmbient
        Lighting.OutdoorAmbient = originalOutdoorAmbient
        Lighting.FogEnd = 10000
    end
end)

-- Visual Toggles (Tracers & ESP Boxes)
local tracersEnabled = false
local boxesEnabled = false
local tracers = {}
local boxes = {}

local function createToggle(parent, name, posY, callback)
    local toggle = Instance.new("TextButton", parent)
    toggle.Size = UDim2.new(0, 200, 0, 30)
    toggle.Position = UDim2.new(0, 10, 0, posY)
    toggle.Text = name .. ": OFF"
    toggle.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    applyCornerRadius(toggle, 4)

    local state = false
    toggle.MouseButton1Click:Connect(function()
        state = not state
        toggle.Text = name .. ": " .. (state and "ON" or "OFF")
        callback(state)
    end)
end

createToggle(visualTab, "Tracers", 50, function(state)
    tracersEnabled = state
    if not state then
        for _, tracer in pairs(tracers) do
            if tracer then
                tracer:Remove()
            end
        end
        tracers = {}
    end
end)

createToggle(visualTab, "ESP Boxes", 90, function(state)
    boxesEnabled = state
    for _, box in pairs(boxes) do box:Destroy() end
    boxes = {}
end)

--[[
    =====================================
            TROLL TAB FEATURES
    =====================================
--]]

-- Gravity Gun
local gravityGun = false
local gravityGunToggle = Instance.new("TextButton", trollTab)
gravityGunToggle.Size = UDim2.new(0, 200, 0, 30)
gravityGunToggle.Position = UDim2.new(0, 10, 0, 10)
gravityGunToggle.Text = "Gravity Gun: OFF"
gravityGunToggle.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
applyCornerRadius(gravityGunToggle, 4)

local heldPart = nil
local bodyVelocity = nil
local bodyGyro = nil

gravityGunToggle.MouseButton1Click:Connect(function()
    gravityGun = not gravityGun
    gravityGunToggle.Text = "Gravity Gun: " .. (gravityGun and "ON" or "OFF")
    
    if not gravityGun and heldPart then
        if bodyVelocity then bodyVelocity:Destroy() end
        if bodyGyro then bodyGyro:Destroy() end
        heldPart = nil
    end
end)

UIS.InputBegan:Connect(function(input)
    if not gravityGun then return end
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        local mouse = LocalPlayer:GetMouse()
        local target = mouse.Target
        if target and target:IsA("BasePart") then
            heldPart = target
            
            bodyVelocity = Instance.new("BodyVelocity")
            bodyVelocity.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
            bodyVelocity.Velocity = Vector3.new()
            bodyVelocity.Parent = heldPart
            
            bodyGyro = Instance.new("BodyGyro")
            bodyGyro.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
            bodyGyro.P = 10000
            bodyGyro.D = 500
            bodyGyro.Parent = heldPart
        end
    elseif input.UserInputType == Enum.UserInputType.MouseButton2 and heldPart then
        if bodyVelocity then
            local mouse = LocalPlayer:GetMouse()
            local direction = (mouse.Hit.Position - heldPart.Position).Unit * 100
            bodyVelocity.Velocity = direction
            task.delay(0.5, function()
                if bodyVelocity then bodyVelocity:Destroy() end
                if bodyGyro then bodyGyro:Destroy() end
                heldPart = nil
            end)
        end
    end
end)

-- Spinbot
local spinbot = false
local spinbotToggle = Instance.new("TextButton", trollTab)
spinbotToggle.Size = UDim2.new(0, 200, 0, 30)
spinbotToggle.Position = UDim2.new(0, 10, 0, 50)
spinbotToggle.Text = "Spinbot: OFF"
spinbotToggle.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
applyCornerRadius(spinbotToggle, 4)

local spinConnection
spinbotToggle.MouseButton1Click:Connect(function()
    spinbot = not spinbot
    spinbotToggle.Text = "Spinbot: " .. (spinbot and "ON" or "OFF")
    
    if spinbot then
        spinConnection = RunService.Heartbeat:Connect(function()
            if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                LocalPlayer.Character.HumanoidRootPart.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.Angles(0, math.rad(30), 0)
            end
        end)
    else
        if spinConnection then
            spinConnection:Disconnect()
            spinConnection = nil
        end
    end
end)

-- Chat Troll
local chatTrollEnabled = false
local chatTrollToggle = Instance.new("TextButton", trollTab)
chatTrollToggle.Size = UDim2.new(0, 200, 0, 30)
chatTrollToggle.Position = UDim2.new(0, 10, 0, 130)  -- Positioned below Touch Fling
chatTrollToggle.Text = "Chat Troll: OFF"
chatTrollToggle.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
applyCornerRadius(chatTrollToggle, 4)

local chatTrollConnection
chatTrollToggle.MouseButton1Click:Connect(function()
    chatTrollEnabled = not chatTrollEnabled
    chatTrollToggle.Text = "Chat Troll: " .. (chatTrollEnabled and "ON" or "OFF")
    
    if chatTrollEnabled then
        -- Start the chat troll script
        local TextChatService = game:GetService("TextChatService")
        
        -- List of buzzwords and memes
        local messages = {
            "Going forward", "Verbal bubble wrap", "Hard stop", "Perfect storm", "Low-hanging fruit",
            "Cutting-edge", "Deep dive", "Disruptive", "Elevate", "Empower", "Leverage", "Synergy",
            "Unleash", "Unlock", "Utilise", "Seamless", "Skyrocket", "SKIBIDI", "LOL", "EZ", "HAHA"
        }
        
        -- Function to get a random message from the list
        local function getRandomMessage()
            return messages[math.random(1, #messages)]
        end
        
        chatTrollConnection = task.spawn(function()
            while chatTrollEnabled do
                -- Function to send a chat message
                local function sendChatMessage(message)
                    if TextChatService then
                        local chatChannel = TextChatService.TextChannels.RBXGeneral
                        if chatChannel then
                            chatChannel:SendAsync(message)
                        else
                            warn("Default chat channel (RBXGeneral) not found!")
                        end
                    else
                        warn("TextChatService is not available!")
                    end
                end
                
                sendChatMessage(getRandomMessage())
                task.wait(10)  -- Wait 10 seconds between messages
            end
        end)
    else
        -- Stop the chat troll script
        if chatTrollConnection then
            task.cancel(chatTrollConnection)
            chatTrollConnection = nil
        end
    end
end)

-- Touch Fling (New Version)
local hiddenfling = false
local flingToggle = Instance.new("TextButton", trollTab)
flingToggle.Size = UDim2.new(0, 200, 0, 30)
flingToggle.Position = UDim2.new(0, 10, 0, 90)
flingToggle.Text = "Touch Fling: OFF"
flingToggle.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
applyCornerRadius(flingToggle, 4)

local flingThread 

if not ReplicatedStorage:FindFirstChild("juisdfj0i32i0eidsuf0iok") then
    local detection = Instance.new("Decal")
    detection.Name = "juisdfj0i32i0eidsuf0iok"
    detection.Parent = ReplicatedStorage
end

local function fling()
    local lp = Players.LocalPlayer
    local c, hrp, vel, movel = nil, nil, nil, 0.1

    while hiddenfling do
        RunService.Heartbeat:Wait()
        c = lp.Character
        hrp = c and c:FindFirstChild("HumanoidRootPart")

        if hrp then
            vel = hrp.Velocity
            hrp.Velocity = vel * 10000 + Vector3.new(0, 10000, 0)
            RunService.RenderStepped:Wait()
            hrp.Velocity = vel
            RunService.Stepped:Wait()
            hrp.Velocity = vel + Vector3.new(0, movel, 0)
            movel = -movel
        end
    end
end

flingToggle.MouseButton1Click:Connect(function()
    hiddenfling = not hiddenfling
    flingToggle.Text = "Touch Fling: " .. (hiddenfling and "ON" or "OFF")

    if hiddenfling then
        flingThread = coroutine.create(fling)
        coroutine.resume(flingThread)
    else
        hiddenfling = false
    end
end)

--[[
    =====================================
            UTILITY TAB FEATURES
    =====================================
--]]

-- Server Hop
local serverHopButton = Instance.new("TextButton", utilityTab)
serverHopButton.Size = UDim2.new(0, 200, 0, 30)
serverHopButton.Position = UDim2.new(0, 10, 0, 10)
serverHopButton.Text = "Server Hop"
serverHopButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
applyCornerRadius(serverHopButton, 4)

serverHopButton.MouseButton1Click:Connect(function()
    local servers = {}
    local req = game:HttpGet("https://games.roblox.com/v1/games/" .. game.PlaceId .. "/servers/Public?sortOrder=Asc&limit=100")
    local data = HttpService:JSONDecode(req)
    
    for _, server in ipairs(data.data) do
        if server.playing < server.maxPlayers and server.id ~= game.JobId then
            table.insert(servers, server.id)
        end
    end
    
    if #servers > 0 then
        TeleportService:TeleportToPlaceInstance(game.PlaceId, servers[math.random(1, #servers)])
    else
        warn("No suitable servers found!")
    end
end)

-- Teleport to Player
local playerDropdown = Instance.new("TextButton", utilityTab)
playerDropdown.Size = UDim2.new(0, 200, 0, 30)
playerDropdown.Position = UDim2.new(0, 10, 0, 50)
playerDropdown.Text = "Select Player"
playerDropdown.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
applyCornerRadius(playerDropdown, 4)

local playerFrame = Instance.new("Frame", utilityTab)
playerFrame.Size = UDim2.new(0, 200, 0, 150)
playerFrame.Position = UDim2.new(0, 10, 0, 85)
playerFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
playerFrame.Visible = false
applyCornerRadius(playerFrame, 4)

local playerList = Instance.new("ScrollingFrame", playerFrame)
playerList.Size = UDim2.new(1, 0, 1, 0)
playerList.BackgroundTransparency = 1
playerList.ScrollBarThickness = 5

local function updatePlayerList()
    playerList:ClearAllChildren()
    
    local yOffset = 0
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            local playerButton = Instance.new("TextButton", playerList)
            playerButton.Size = UDim2.new(1, -10, 0, 30)
            playerButton.Position = UDim2.new(0, 5, 0, yOffset)
            playerButton.Text = player.Name
            playerButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
            playerButton.TextColor3 = Color3.new(1, 1, 1)
            applyCornerRadius(playerButton, 4)
            
            playerButton.MouseButton1Click:Connect(function()
                if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                    LocalPlayer.Character:MoveTo(player.Character.HumanoidRootPart.Position)
                end
                playerFrame.Visible = false
            end)
            
            yOffset = yOffset + 35
        end
    end
    
    playerList.CanvasSize = UDim2.new(0, 0, 0, yOffset)
end

playerDropdown.MouseButton1Click:Connect(function()
    playerFrame.Visible = not playerFrame.Visible
    if playerFrame.Visible then
        updatePlayerList()
    end
end)

Players.PlayerAdded:Connect(updatePlayerList)
Players.PlayerRemoving:Connect(updatePlayerList)

-- Rejoin Button
local rejoinButton = Instance.new("TextButton", utilityTab)
rejoinButton.Size = UDim2.new(0, 200, 0, 30)
rejoinButton.Position = UDim2.new(0, 10, 0, 250)
rejoinButton.Text = "Rejoin Game"
rejoinButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
applyCornerRadius(rejoinButton, 4)

rejoinButton.MouseButton1Click:Connect(function()
    TeleportService:Teleport(game.PlaceId)
end)

--[[
    =====================================
            ESP/TRACERS SYSTEM
    =====================================
--]]

-- Store player connections to clean up properly
local playerConnections = {}

local function updateTracers()
    if not tracersEnabled then return end
    
    local localChar = LocalPlayer.Character
    local localHrp = localChar and localChar:FindFirstChild("HumanoidRootPart")
    if not localHrp then return end
    
    local localScreenPos, localVisible = Camera:WorldToViewportPoint(localHrp.Position)
    if not localVisible then return end
    
    local tracerStart = Vector2.new(localScreenPos.X, localScreenPos.Y)
    
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            if not tracers[player] then
                tracers[player] = Drawing.new("Line")
                tracers[player].Visible = false
                tracers[player].Thickness = 2
                tracers[player].Color = Color3.new(1, 1, 1)
                
                playerConnections[player] = player.CharacterAdded:Connect(function(character)
                    local hrp = character:WaitForChild("HumanoidRootPart", 5)
                    if hrp and tracers[player] then
                        tracers[player].Visible = tracersEnabled
                    end
                end)
                
                player.AncestryChanged:Connect(function()
                    if not player:IsDescendantOf(Players) then
                        if tracers[player] then
                            tracers[player]:Remove()
                            tracers[player] = nil
                        end
                        if playerConnections[player] then
                            playerConnections[player]:Disconnect()
                            playerConnections[player] = nil
                        end
                    end
                end)
            end
            
            if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                local hrp = player.Character.HumanoidRootPart
                local screenPos, visible = Camera:WorldToViewportPoint(hrp.Position)
                
                if tracers[player] then
                    tracers[player].Visible = visible and tracersEnabled
                    if visible then
                        tracers[player].From = tracerStart
                        tracers[player].To = Vector2.new(screenPos.X, screenPos.Y)
                    end
                end
            elseif tracers[player] then
                tracers[player].Visible = false
            end
        end
    end
    
    for player, tracer in pairs(tracers) do
        if not Players:FindFirstChild(player.Name) then
            if tracer then
                tracer:Remove()
            end
            tracers[player] = nil
            if playerConnections[player] then
                playerConnections[player]:Disconnect()
                playerConnections[player] = nil
            end
        end
    end
end

RunService.RenderStepped:Connect(function()
    if tracersEnabled then
        updateTracers()
    end

    if boxesEnabled then
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                if not player.Character:FindFirstChild("MinHubBox") then
                    local box = Instance.new("BoxHandleAdornment")
                    box.Name = "MinHubBox"
                    box.Adornee = player.Character
                    box.AlwaysOnTop = true
                    box.ZIndex = 5
                    box.Size = Vector3.new(4, 6, 2)
                    box.Transparency = 0.5
                    box.Color3 = Color3.new(1, 1, 1)
                    box.Parent = player.Character
                    table.insert(boxes, box)
                end
            end
        end
    end
end)
