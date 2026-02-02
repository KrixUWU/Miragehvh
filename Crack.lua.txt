-- lvzz crack https://www.youtube.com/@Script_house/videos
local EXPIRY_DAY, EXPIRY_MONTH, EXPIRY_YEAR = 9, 99, 9999
local function GetBerlinTime()
    local utc = os.time(os.date("!*t"))
    return os.date("*t", utc)
end
local berlinNow = GetBerlinTime()
if berlinNow.year > EXPIRY_YEAR or 
   (berlinNow.year == EXPIRY_YEAR and berlinNow.month > EXPIRY_MONTH) or
   (berlinNow.year == EXPIRY_YEAR and berlinNow.month == EXPIRY_MONTH and berlinNow.day > EXPIRY_DAY) then
    return
end

-- ========== ANTI-DUMP SYSTEM ==========
-- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
local _G_DUMP_PROTECT = _G_DUMP_PROTECT or {}
local _ANTI_DUMP_ACTIVE = true

-- Invalid code Invalid code Invalid code
local function _ad_rstr(l)
    local c = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789_"
    local s = ""
    for i = 1, l do
        local r = math.random(1, #c)
        s = s .. c:sub(r, r)
    end
    return s
end

-- Invalid code Invalid code Invalid code Invalid code Invalid code
local _JUNK_TABLES = {}
local _JUNK_FUNCS = {}
local _JUNK_STRINGS = {}
local _JUNK_NESTED = {}

-- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
local _FAKE_PATTERNS = {
    "local function %s() local a = game:GetService('Players') return a end",
    "local %s = function(x) return x * 2 + math.random() end",
    "local %s = {enabled = true, value = %d, key = '%s'}",
    "getgenv().%s = function() return '%s' end",
    "local %s = Instance.new('Part') %s.Name = '%s'",
    "local %s = game:GetService('RunService').Heartbeat:Connect(function() end)",
    "local %s = coroutine.create(function() while true do task.wait() end end)",
    "local %s = setmetatable({}, {__index = function() return '%s' end})",
}

-- Invalid code Invalid code Invalid code Invalid code Invalid code
task.spawn(function()
    -- Invalid code 500 Invalid code Invalid code
    for i = 1, 500 do
        local tbl = {}
        for j = 1, math.random(50, 200) do
            local key = _ad_rstr(math.random(8, 32))
            local valType = math.random(1, 5)
            if valType == 1 then
                tbl[key] = math.random(-999999, 999999)
            elseif valType == 2 then
                tbl[key] = _ad_rstr(math.random(16, 128))
            elseif valType == 3 then
                tbl[key] = math.random() > 0.5
            elseif valType == 4 then
                tbl[key] = function() return _ad_rstr(32) end
            else
                tbl[key] = {_ad_rstr(16), math.random(), _ad_rstr(16)}
            end
        end
        _JUNK_TABLES[_ad_rstr(16)] = tbl
        if i % 50 == 0 then task.wait() end
    end
    
    -- Invalid code 1000 Invalid code Invalid code (Invalid code Invalid code)
    for i = 1, 1000 do
        local pattern = _FAKE_PATTERNS[math.random(1, #_FAKE_PATTERNS)]
        local fakeCode = string.format(pattern, 
            _ad_rstr(math.random(8, 24)),
            math.random(1, 99999),
            _ad_rstr(math.random(8, 16)),
            _ad_rstr(math.random(8, 16)),
            _ad_rstr(math.random(8, 16))
        )
        _JUNK_STRINGS[i] = fakeCode
        if i % 100 == 0 then task.wait() end
    end
    
    -- Invalid code Invalid code Invalid code (Invalid code 10)
    for i = 1, 100 do
        local root = {}
        local current = root
        for depth = 1, 10 do
            current[_ad_rstr(8)] = {}
            current[_ad_rstr(8)] = _ad_rstr(64)
            current[_ad_rstr(8)] = math.random(-999999, 999999)
            current[_ad_rstr(8)] = function() return _ad_rstr(32) end
            current = current[_ad_rstr(8)] or {}
        end
        _JUNK_NESTED[i] = root
        if i % 20 == 0 then task.wait() end
    end
    
    -- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
    for i = 1, 200 do
        local name = "_" .. _ad_rstr(math.random(16, 32))
        getgenv()[name] = _JUNK_TABLES[next(_JUNK_TABLES)] or _ad_rstr(128)
        if i % 50 == 0 then task.wait() end
    end
    
    -- Invalid code Invalid code Instance Invalid code
    for i = 1, 100 do
        local fakeName = _ad_rstr(16)
        _G_DUMP_PROTECT[fakeName] = {
            FireServer = function() end,
            InvokeServer = function() end,
            Connect = function() return {Disconnect = function() end} end,
            Name = _ad_rstr(16),
            Parent = game,
            ClassName = "RemoteEvent"
        }
        if i % 25 == 0 then task.wait() end
    end
end)

-- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
task.spawn(function()
    while _ANTI_DUMP_ACTIVE do
        task.wait(5)
        -- Invalid code Invalid code Invalid code Invalid code Invalid code 5 Invalid code
        for i = 1, 50 do
            local key = _ad_rstr(math.random(12, 24))
            _JUNK_TABLES[key] = {
                [_ad_rstr(8)] = math.random(-999999, 999999),
                [_ad_rstr(8)] = _ad_rstr(math.random(32, 128)),
                [_ad_rstr(8)] = function() return _ad_rstr(16) end
            }
        end
        -- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
        local count = 0
        for k in pairs(_JUNK_TABLES) do
            count = count + 1
            if count > 600 then
                _JUNK_TABLES[k] = nil
            end
        end
    end
end)
-- ========== END ANTI-DUMP ==========

-- ========== AI PREDICTION SYSTEM ==========
-- Invalid code Invalid code + Invalid code Invalid code + Invalid code Invalid code
local AI_PRED = {
    history = {}, -- Invalid code Invalid code Invalid code Invalid code
    patterns = {}, -- Invalid code Invalid code Invalid code
    peekData = {}, -- Invalid code Invalid code Invalid code
    HISTORY_SIZE = 30, -- Invalid code Invalid code Invalid code
    PEEK_THRESHOLD = 15, -- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
    STRAFE_THRESHOLD = 8, -- Invalid code Invalid code Invalid code Invalid code
    peekDetect = true, -- Invalid code Invalid code
    strafeDetect = true, -- Invalid code Invalid code
}

-- Invalid code Invalid code Invalid code Invalid code
local function AI_AddHistory(playerName, pos, vel, time)
    if not AI_PRED.history[playerName] then
        AI_PRED.history[playerName] = {}
    end
    local h = AI_PRED.history[playerName]
    table.insert(h, {pos = pos, vel = vel, time = time})
    -- Invalid code Invalid code
    while #h > AI_PRED.HISTORY_SIZE do
        table.remove(h, 1)
    end
end

-- Invalid code Invalid code Invalid code
local function AI_AnalyzePattern(playerName)
    local h = AI_PRED.history[playerName]
    if not h or #h < 10 then return nil end
    
    local pattern = {
        isPeeking = false,
        isStrafing = false,
        peekDirection = nil,
        strafeDirection = nil,
        avgSpeed = 0,
        directionChanges = 0,
        predictedPos = nil,
        confidence = 0
    }
    
    -- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
    local totalSpeed = 0
    local dirChanges = 0
    local lastDir = nil
    
    for i = 2, #h do
        local prev = h[i-1]
        local curr = h[i]
        local moveDir = (curr.pos - prev.pos).Unit
        local speed = (curr.pos - prev.pos).Magnitude / math.max(0.001, curr.time - prev.time)
        totalSpeed = totalSpeed + speed
        
        if lastDir then
            local dot = moveDir:Dot(lastDir)
            if dot < 0.5 then -- Invalid code Invalid code Invalid code
                dirChanges = dirChanges + 1
            end
        end
        lastDir = moveDir
    end
    
    pattern.avgSpeed = totalSpeed / (#h - 1)
    pattern.directionChanges = dirChanges
    
    -- Invalid code Invalid code (Invalid code Invalid code Invalid code-Invalid code Invalid code)
    if #h >= 5 then
        local recent = h[#h]
        local older = h[#h - 4]
        local recentVel = recent.vel
        local olderVel = older.vel
        
        -- Invalid code Invalid code Invalid code Invalid code - Invalid code Invalid code
        local velChange = (recentVel - olderVel).Magnitude
        if velChange > AI_PRED.PEEK_THRESHOLD then
            pattern.isPeeking = true
            pattern.peekDirection = recentVel.Unit
            pattern.confidence = math.min(1, velChange / 30)
        end
    end
    
    -- Invalid code Invalid code (A-D spam)
    if dirChanges >= 3 and pattern.avgSpeed > 5 then
        pattern.isStrafing = true
        -- Invalid code Invalid code Invalid code Invalid code Invalid code
        local lastVel = h[#h].vel
        pattern.strafeDirection = Vector3.new(-lastVel.X, 0, -lastVel.Z).Unit -- Invalid code Invalid code
        pattern.confidence = math.min(1, dirChanges / 6)
    end
    
    AI_PRED.patterns[playerName] = pattern
    return pattern
end

-- Invalid code Invalid code Invalid code AI
local function AI_PredictPosition(playerName, currentPos, currentVel, predTime)
    local pattern = AI_PRED.patterns[playerName]
    local h = AI_PRED.history[playerName]
    
    -- Invalid code Invalid code
    local predicted = currentPos + currentVel * predTime
    
    if not pattern or not h or #h < 5 then
        return predicted, 0.5 -- Invalid code Invalid code
    end
    
    -- Invalid code Invalid code Invalid code - Invalid code Invalid code Invalid code Invalid code
    if pattern.isPeeking and pattern.peekDirection then
        -- Invalid code Invalid code Invalid code-Invalid code Invalid code - Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
        local peekOffset = pattern.peekDirection * (pattern.avgSpeed * predTime * 1.5)
        predicted = currentPos + peekOffset
        return predicted, pattern.confidence
    end
    
    -- Invalid code Invalid code Invalid code - Invalid code Invalid code Invalid code
    if pattern.isStrafing and pattern.strafeDirection then
        -- A-D spam - Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
        local strafeOffset = pattern.strafeDirection * (pattern.avgSpeed * predTime * 0.5)
        predicted = currentPos + strafeOffset
        return predicted, pattern.confidence
    end
    
    -- Invalid code Invalid code Invalid code Invalid code Invalid code
    if #h >= 10 then
        -- Invalid code Invalid code 10 Invalid code Invalid code Invalid code
        local sumVel = Vector3.zero
        for i = #h - 9, #h do
            sumVel = sumVel + h[i].vel
        end
        local avgVel = sumVel / 10
        
        -- Invalid code Invalid code (Invalid code Invalid code Invalid code Invalid code)
        local weightedVel = (currentVel * 0.7) + (avgVel * 0.3)
        predicted = currentPos + weightedVel * predTime
        return predicted, 0.7
    end
    
    return predicted, 0.5
end

-- Invalid code Invalid code Invalid code (Invalid code Invalid code Invalid code)
local function AI_DetectPeekStart(playerName, currentPos, currentVel)
    local h = AI_PRED.history[playerName]
    if not h or #h < 3 then return false, nil end
    
    local prev = h[#h - 2]
    local curr = h[#h]
    
    -- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
    local wasStill = prev.vel.Magnitude < 2
    local isMoving = currentVel.Magnitude > 5
    
    if wasStill and isMoving then
        -- Invalid code Invalid code! Invalid code Invalid code Invalid code
        local peekDir = currentVel.Unit
        local predictedPeekPos = currentPos + peekDir * 8 -- 8 studs Invalid code
        return true, predictedPeekPos
    end
    
    -- Invalid code Invalid code Invalid code (Invalid code Invalid code-Invalid code Invalid code)
    if #h >= 5 then
        local oldDir = (h[#h-2].pos - h[#h-4].pos)
        local newDir = (currentPos - h[#h-2].pos)
        if oldDir.Magnitude > 0.1 and newDir.Magnitude > 0.1 then
            local dot = oldDir.Unit:Dot(newDir.Unit)
            if dot < 0 then -- Invalid code Invalid code Invalid code Invalid code Invalid code 90 Invalid code
                local predictedPos = currentPos + newDir.Unit * 6
                return true, predictedPos
            end
        end
    end
    
    return false, nil
end

-- Invalid code Invalid code Invalid code Invalid code
local function AI_GetBestPrediction(playerName, headPos, vel, ping)
    local predTime = ping * 1.2 -- Invalid code Invalid code Invalid code
    
    -- Invalid code Invalid code Invalid code
    AI_AddHistory(playerName, headPos, vel, tick())
    
    -- Invalid code Invalid code
    AI_AnalyzePattern(playerName)
    
    -- Invalid code Invalid code Invalid code (Invalid code Invalid code)
    local isPeekStart, peekPos = false, nil
    if AI_PRED.peekDetect then
        isPeekStart, peekPos = AI_DetectPeekStart(playerName, headPos, vel)
        if isPeekStart and peekPos then
            return peekPos, 0.9, "PEEK"
        end
    end
    
    -- Invalid code AI Invalid code
    local predicted, confidence = AI_PredictPosition(playerName, headPos, vel, predTime)
    
    local pattern = AI_PRED.patterns[playerName]
    local mode = "TRACK"
    if pattern then
        if pattern.isStrafing then mode = "STRAFE"
        elseif pattern.isPeeking then mode = "PEEK" end
    end
    
    return predicted, confidence, mode
end

-- ========== AI VISUAL SYSTEM ==========
local AI_VISUALS = {
    boxes = {},
    traces = {},
    predictions = {}
}

-- Invalid code Invalid code Invalid code Invalid code Invalid code
local function AI_CreateBox(playerName, position, color, mode)
    -- Invalid code Invalid code
    if AI_VISUALS.boxes[playerName] then
        pcall(function() AI_VISUALS.boxes[playerName]:Destroy() end)
    end
    
    local box = Instance.new("BoxHandleAdornment")
    box.Name = "AIBox_" .. playerName
    box.Size = Vector3.new(4, 6, 4)
    box.Color3 = color
    box.Transparency = 0.7
    box.AlwaysOnTop = true
    box.ZIndex = 5
    box.Adornee = workspace.Terrain
    box.CFrame = CFrame.new(position)
    box.Parent = game:GetService("CoreGui")
    
    AI_VISUALS.boxes[playerName] = box
    
    -- Invalid code Invalid code 0.1 Invalid code
    task.delay(0.1, function()
        pcall(function() box:Destroy() end)
    end)
end

-- Invalid code Invalid code Invalid code (Invalid code Invalid code)
local function AI_CreateTrace(playerName, fromPos, toPos, color)
    -- Invalid code Invalid code
    if AI_VISUALS.traces[playerName] then
        pcall(function() AI_VISUALS.traces[playerName]:Destroy() end)
    end
    
    local distance = (toPos - fromPos).Magnitude
    local midPoint = (fromPos + toPos) / 2
    
    local trace = Instance.new("Part")
    trace.Name = "AITrace_" .. playerName
    trace.Anchored = true
    trace.CanCollide = false
    trace.Material = Enum.Material.Neon
    trace.Color = color
    trace.Size = Vector3.new(0.1, 0.1, distance)
    trace.CFrame = CFrame.lookAt(midPoint, toPos)
    trace.Transparency = 0.3
    trace.Parent = workspace
    
    AI_VISUALS.traces[playerName] = trace
    
    -- Invalid code Invalid code 0.1 Invalid code
    task.delay(0.1, function()
        pcall(function() trace:Destroy() end)
    end)
end

-- Invalid code Invalid code Invalid code
local function AI_CreatePredictionPoint(playerName, position, confidence)
    -- Invalid code Invalid code
    if AI_VISUALS.predictions[playerName] then
        pcall(function() AI_VISUALS.predictions[playerName]:Destroy() end)
    end
    
    local point = Instance.new("Part")
    point.Name = "AIPred_" .. playerName
    point.Shape = Enum.PartType.Ball
    point.Anchored = true
    point.CanCollide = false
    point.Material = Enum.Material.Neon
    -- Invalid code Invalid code Invalid code Invalid code
    if confidence > 0.8 then
        point.Color = Color3.fromRGB(0, 255, 0) -- Invalid code - Invalid code
    elseif confidence > 0.6 then
        point.Color = Color3.fromRGB(255, 255, 0) -- Invalid code - Invalid code
    else
        point.Color = Color3.fromRGB(255, 0, 0) -- Invalid code - Invalid code
    end
    point.Size = Vector3.new(0.5, 0.5, 0.5)
    point.Position = position
    point.Transparency = 0.2
    point.Parent = workspace
    
    AI_VISUALS.predictions[playerName] = point
    
    -- Invalid code Invalid code 0.1 Invalid code
    task.delay(0.1, function()
        pcall(function() point:Destroy() end)
    end)
end

-- Invalid code Invalid code Invalid code Invalid code Invalid code
local function AI_ShowVisuals(playerName, realPos, predictedPos, confidence, mode)
    -- Invalid code Invalid code Invalid code
    local color = Color3.fromRGB(255, 255, 255)
    if mode == "PEEK" then
        color = Color3.fromRGB(255, 0, 0) -- Invalid code - Invalid code
    elseif mode == "STRAFE" then
        color = Color3.fromRGB(255, 165, 0) -- Invalid code - Invalid code
    elseif mode == "TRACK" then
        color = Color3.fromRGB(0, 255, 255) -- Invalid code - Invalid code
    end
    
    -- Invalid code Invalid code Invalid code Invalid code
    AI_CreateBox(playerName, realPos, color, mode)
    
    -- Invalid code Invalid code Invalid code Invalid code Invalid code
    AI_CreateTrace(playerName, realPos, predictedPos, color)
    
    -- Invalid code Invalid code
    AI_CreatePredictionPoint(playerName, predictedPos, confidence)
end

-- Invalid code Invalid code Invalid code
local function AI_ClearVisuals()
    for _, box in pairs(AI_VISUALS.boxes) do
        pcall(function() box:Destroy() end)
    end
    for _, trace in pairs(AI_VISUALS.traces) do
        pcall(function() trace:Destroy() end)
    end
    for _, pred in pairs(AI_VISUALS.predictions) do
        pcall(function() pred:Destroy() end)
    end
    AI_VISUALS.boxes = {}
    AI_VISUALS.traces = {}
    AI_VISUALS.predictions = {}
end
-- ========== END AI VISUAL ==========

-- ========== END AI PREDICTION ==========

local UIS = game:GetService("UserInputService")
local RS = game:GetService("RunService")
local Light = game:GetService("Lighting")
local Plrs = game:GetService("Players")
local WS = game:GetService("Workspace")
local HTTP = game:GetService("HttpService")
local TS = game:GetService("TextService")
local LP = Plrs.LocalPlayer

-- Theme
local T = {
    Main = Color3.fromRGB(12, 12, 12),
    Group = Color3.fromRGB(19, 19, 19),
    Stroke = Color3.fromRGB(45, 45, 45),
    Accent = Color3.fromRGB(168, 247, 50),
    Text = Color3.fromRGB(220, 220, 220),
    Dim = Color3.fromRGB(140, 140, 140),
    Font = Enum.Font.Code,
    Dark = Color3.fromRGB(25, 25, 25),
    Darker = Color3.fromRGB(20, 20, 20),
    Border = Color3.fromRGB(60, 60, 60)
}

-- Config
local Cfg = {
    Folder = "Arcanum", 
    List = {}, 
    Selected = "", 
    FS = (writefile and readfile and isfile and listfiles and makefolder and isfolder) ~= nil
}

-- Settings
local S = {
    menuKey = Enum.KeyCode.Delete,
    rbEnabled = false, rbResolver = false, rbResolverMode = "Safe", rbPrediction = true, rbTeamCheck = true,
    rbAutoFire = true, rbHitbox = "Head", rbMaxDist = 500, rbFireRate = 0,
    rbPredMulti = 1.0, btEnabled = true, btTime = 0,
    rbNoAir = true, rbWallCheck = true,
    rbSmartAim = true,
    rbAirShoot = false, rbBodyAimHP = 50,
    rbPredMode = "Default", -- "Default" Invalid code "Beta AI"
    -- BETA AI SETTINGS
    aiHistorySize = 30, -- Invalid code Invalid code Invalid code
    aiConfThreshold = 60, -- Invalid code Invalid code (%)
    aiPeekDetect = true, -- Invalid code Invalid code
    aiStrafeDetect = true, -- Invalid code Invalid code
    aiVisualBox = false, -- Invalid code Invalid code
    aiVisualTrace = false, -- Invalid code Invalid code
    -- DOUBLE TAP
    dtEnabled = false, dtKey = Enum.KeyCode.E, dtDist = 6, dtMode = "Defensive",
    dtAuto = false, dtAutoDelay = 200,
    fdEnabled = false, fdTeamCheck = true,
    fdKey = Enum.KeyCode.X, fdLockKey = Enum.KeyCode.V,
    -- EXPLOIT POSITION
    epEnabled = false, epKey = Enum.KeyCode.C, epDist = 3,
    -- INFINITY JUMP
    ijEnabled = false, ijKey = Enum.KeyCode.Space,
    -- BARREL EXTEND
    beEnabled = false, beKey = Enum.KeyCode.G, beMode = "Hold", beDist = 5,
    -- AIMVIEW
    avEnabled = false, avColor = Color3.fromRGB(255, 0, 0),
    -- HITMARKER & KILL EFFECT
    hmEnabled = true, hmColor = Color3.fromRGB(168, 247, 50),
    keEnabled = true, keColor = Color3.fromRGB(255, 255, 255),
    -- CUSTOM HIT SOUND
    hsEnabled = false, hsSoundId = "139894735376184", hsVolume = 100, hsSelected = "Default",
    -- FORTNITE DAMAGE
    fdmgEnabled = true, fdmgColor = Color3.fromRGB(255, 255, 255),
    -- AI PEEK V4 SETTINGS
    apEnabled = false, apKey = Enum.KeyCode.LeftAlt, apMode = "Hold",
    apShowPoints = false, apTeamCheck = true, apESP = false,
    apHeight = 2.0, apSpeed = 200, apCooldown = 0.1, apRange = 80, apPeekDist = 8,
    -- GHOST PEEK SETTINGS
    gpEnabled = true, gpKey = Enum.KeyCode.Q, gpMode = "Hold",
    gpRange = 100, gpPeekDist = 8, gpHeight = 3, gpQuality = 50,
    gpTeamCheck = true, gpAutoshoot = true,
    -- VISUALS
    wmVisible = true, hkVisible = true, timeVisible = true, selectedModel = "None",
    bloomEnabled = false, bloomIntensity = 1.5, bloomSize = 40, bloomThreshold = 0.7,
    blurEnabled = false, blurSize = 10,
    colorEnabled = false, ccBrightness = 0, ccContrast = 0.1, ccSaturation = 0.2, ccTintColor = Color3.new(1, 1, 1),
    sunRaysEnabled = false, sunRaysIntensity = 0.15, sunRaysSpread = 0.8,
    fogEnabled = false, fogStart = 0, fogEnd = 500, fogColor = Color3.new(0.5, 0.5, 0.5),
    -- CUSTOM SKYBOX
    skyboxEnabled = false, skyboxId = "139989099041467", skyboxStarsEnabled = true, skyboxStarsCount = 20, selectedSkybox = "None",
    -- MISC
    bhEnabled = false, bhKey = Enum.KeyCode.F, bhGroundSpeed = 35, bhAirSpeed = 39,
    wbEnabled = false, rcEnabled = false, tpEnabled = false, tpCTKey = Enum.KeyCode.One, tpTKey = Enum.KeyCode.Two,
    -- DEBUG CONSOLE
    dcEnabled = false, dcKey = Enum.KeyCode.P, dcVisible = false, dcNotify = false,
    -- ANIM BREAKER
    abEnabled = false, abKey = Enum.KeyCode.B,
    -- AIMVIEW
    avTransparency = 0.3,
    -- ACCENT COLOR
    accentColor = "Green",
    -- HIT LOGGER
    hlEnabled = true, hlMaxLogs = 8
}

-- Runtime
local R = {
    gui = nil, main = nil, hkFrame = nil, wmFrame = nil, timeFrame = nil, visible = true,
    drag = false, dragStart = nil, startPos = nil,
    conns = {}, fx = {}, hotkeys = {},
    lastDeff = 0, cfgDropdown = nil, cfgDropdownList = nil,
    fdTarget = nil, fdCrouch = false, fdLock = false,
    bloomEffect = nil, blurEffect = nil, colorEffect = nil, sunRaysEffect = nil,
    skyboxOriginal = nil, skyboxStars = {},
    fdIdleAnim = nil, fdWalkAnim = nil,
    rbLast = 0,
    -- HIT AIR RUNTIME
    hitAirPart = nil, hitAirActive = false,
    -- EXPLOIT POSITION RUNTIME
    epDecoy = nil, epActive = false, epOriginalPos = nil, epOriginalNeckC0 = nil, epHealthConn = nil,
    -- INFINITY JUMP RUNTIME
    ijPart = nil, ijCanJump = true, ijWasGrounded = true,
    -- BARREL EXTEND RUNTIME
    beActive = false, beDecoy = nil, beUpdateConn = nil,
    -- AIMVIEW RUNTIME
    avLine = nil, avTarget = nil,
    -- DOUBLE TAP RUNTIME
    dtLastPeek = 0, dtAutoLast = 0,
    -- DEBUG CONSOLE RUNTIME
    dcFrame = nil, dcLogs = {}, dcMaxLogs = 50, dcNotifies = {},
    -- ANIM BREAKER RUNTIME
    abActive = false, abConn = nil,
    -- AI PEEK V4 RUNTIME
    apActive = false, apTeleporting = false, apLastTP = 0,
    apPoints = {}, apOriginalCFrame = nil, apPointCount = 0,
    apSavedOrigin = nil,
    -- GHOST PEEK RUNTIME
    gpActive = false, gpInPeek = false, gpLastShot = 0,
    -- HIT LOGGER RUNTIME
    hlFrame = nil, hlLogIndex = 0,
    -- AI PREDICTION RUNTIME
    aiPredData = {}, -- Invalid code Invalid code Invalid code Invalid code
    
    mainConn = nil, running = true, sliderDrag = nil,
    bhConn = nil, bhOrigSpeed = 16, bhInAir = false,
    bhLastReset = 0, bhResetting = false,
    bhLastPos = nil, bhPosCheckTime = 0, bhCircling = false,
    myChar = nil, myHRP = nil, myHead = nil, myHum = nil,
    fireShot = nil, fireShotTime = 0,
    playerData = {}, playerDataTime = 0,
    cam = nil,
    -- ACCENT ELEMENTS (Invalid code Invalid code Invalid code)
    accentElements = {}
}

-- Invalid code
local FD_DIST = 200
local FLOOR = math.floor
local CLAMP = math.clamp
local TICK = tick
local V3_NEW = Vector3.new
local V3_ZERO = Vector3.zero
local COS = math.cos
local SIN = math.sin
local RAD = math.rad
local MAX = math.max
local MIN = math.min
local CEIL = math.ceil
local HUGE = math.huge
local ABS = math.abs
local SQRT = math.sqrt

-- RaycastParams
local RayP = RaycastParams.new()
RayP.FilterType = Enum.RaycastFilterType.Exclude

-- AI Peek RayParams
local APRayP = RaycastParams.new()
APRayP.FilterType = Enum.RaycastFilterType.Exclude

-- Forward declarations
local UpdateHotkeyList, ApplySettings, AddHitLog, TrackShot, SetupKillTracking, CreateHitLogger

-- ========== SAFE FIRE SERVER ==========
local GLOBAL_FIRE_COOLDOWN = 1.5  -- 1500Invalid code Invalid code Invalid code
local lastFireServer = 0

local function SafeFireServer(fs, ...)
    local now = TICK()
    if now - lastFireServer < GLOBAL_FIRE_COOLDOWN then return false end
    lastFireServer = now
    pcall(fs.FireServer, fs, ...)
    return true
end

-- ========== GET FIRE SHOT ==========
local function GetFireShot()
    local now = TICK()
    if R.fireShot and R.fireShot.Parent and now - R.fireShotTime < 5 then return R.fireShot end
    if not R.myChar then CacheChar() end
    if not R.myChar then return nil end
    for _, child in ipairs(R.myChar:GetChildren()) do
        if child:IsA("Tool") then
            local remotes = child:FindFirstChild("Remotes")
            if remotes then
                local fs = remotes:FindFirstChild("FireShot") or remotes:FindFirstChild("fireShot")
                if fs then R.fireShot, R.fireShotTime = fs, now return fs end
            end
        end
    end
    return nil
end

-- ========== CACHE CHARACTER ==========
local function CacheChar()
    local c = LP.Character
    if c then
        R.myChar, R.myHRP = c, c:FindFirstChild("HumanoidRootPart")
        R.myHead, R.myHum = c:FindFirstChild("Head"), c:FindFirstChildOfClass("Humanoid")
    else
        R.myChar, R.myHRP, R.myHead, R.myHum = nil, nil, nil, nil
    end
    R.cam = WS.CurrentCamera
end

-- ========== WALLBANG HELPER DATA ==========
local wallbangWalls = {
    {name = "hamik", pos = Vector3.new(97.46, 36.63, -2.82)},
    {name = "hamik", pos = Vector3.new(97.46, 36.63, 0.46)},
    {name = "hamik", pos = Vector3.new(97.46, 36.65, -1.18)},
    {name = "hamik", pos = Vector3.new(97.20, 38.47, -1.18)},
    {name = "hamik", pos = Vector3.new(96.52, 41.89, -1.18)},
    {name = "hamik", pos = Vector3.new(98.34, 41.89, -1.18)},
    {name = "hamik", pos = Vector3.new(97.46, 34.80, -1.18)},
    {name = "hamik", pos = Vector3.new(152.46, 35.57, -6.88)},
    {name = "hamik", pos = Vector3.new(150.34, 33.86, 134.91)},
    {name = "hamik", pos = Vector3.new(153.05, 33.91, 134.90)},
    {name = "hamik", pos = Vector3.new(151.95, 33.74, 134.93)},
    {name = "hamik", pos = Vector3.new(161.36, 40.47, 153.08)},
    {name = "hamik", pos = Vector3.new(108.23, 34.15, 94.53)},
    {name = "hamik", pos = Vector3.new(108.73, 38.13, 93.78)},
    {name = "hamik", pos = Vector3.new(71.21, 37.96, -18.74)},
    {name = "hamik", pos = Vector3.new(71.21, 43.09, -18.96)},
    {name = "Part", pos = Vector3.new(96.63, 41.21, -3.74)},
    {name = "Part", pos = Vector3.new(96.63, 41.21, 1.38)},
    {name = "Part", pos = Vector3.new(205.19, 38.08, 187.89)},
    {name = "Part", pos = Vector3.new(209.46, 30.75, 180.54)},
    {name = "Part", pos = Vector3.new(153.66, 33.63, 135.62)},
    {name = "Part", pos = Vector3.new(149.04, 33.91, 135.60)},
    {name = "Part", pos = Vector3.new(108.23, 30.38, 83.15)},
    {name = "Part", pos = Vector3.new(108.23, 38.33, 72.24)},
    {name = "Part", pos = Vector3.new(108.23, 43.15, 88.36)},
    {name = "Part", pos = Vector3.new(108.74, 38.13, 72.24)},
    {name = "Part", pos = Vector3.new(108.73, 42.61, 83.07)},
    {name = "Part", pos = Vector3.new(108.73, 30.48, 83.07)},
    {name = "Part", pos = Vector3.new(109.42, 30.04, 93.90)},
    {name = "Part", pos = Vector3.new(88.31, 34.68, 122.17)},
    {name = "Part", pos = Vector3.new(87.91, 31.80, 122.17)},
    {name = "Part", pos = Vector3.new(87.28, 33.18, 118.75)},
    {name = "Part", pos = Vector3.new(87.28, 33.18, 120.63)},
    {name = "Part", pos = Vector3.new(88.31, 34.68, 98.91)},
    {name = "Part", pos = Vector3.new(87.91, 32.32, 100.28)},
    {name = "Part", pos = Vector3.new(65.08, 31.62, 91.62)},
    {name = "Part", pos = Vector3.new(127.95, 46.97, 0.99)},
    {name = "Part", pos = Vector3.new(67.02, 37.96, -18.74)},
    {name = "paletka", pos = Vector3.new(61.81, 35.08, 90.42)},
    {name = "paletka", pos = Vector3.new(61.81, 39.58, 91.62)},
    {name = "paletka", pos = Vector3.new(143.97, 32.34, 199.64)},
    {name = "paletka", pos = Vector3.new(141.99, 32.43, 200.56)},
    {name = "paletka", pos = Vector3.new(139.40, 32.34, 198.31)},
    {name = "paletka", pos = Vector3.new(142.04, 32.34, 203.99)},
    {name = "paletka", pos = Vector3.new(145.88, 32.43, 198.74)},
    {name = "paletka", pos = Vector3.new(148.51, 32.34, 200.97)},
    {name = "paletka", pos = Vector3.new(145.86, 32.34, 195.29)},
    {name = "paletka", pos = Vector3.new(143.96, 36.87, 199.64)},
    {name = "paletka", pos = Vector3.new(143.97, 27.64, 199.64)},
    {name = "nowallbang1", pos = Vector3.new(67.38, 43.09, -18.96)},
}

getgenv().wallbangCache = getgenv().wallbangCache or {}
getgenv().collisionCache = getgenv().collisionCache or {}
getgenv().wallbangMapCache = getgenv().wallbangMapCache or {}

-- ========== WALLBANG FUNCTIONS ==========
local wallObjectCache = {}
local wallCacheTime = 0
local wallCacheBuilding = false

local function findWallObject(name, targetPos)
    local now = TICK()
    -- Invalid code Invalid code Invalid code 60 Invalid code (Invalid code Invalid code Invalid code Invalid code)
    if now - wallCacheTime > 60 then
        wallObjectCache = {}
        wallCacheTime = now
    end
    
    local cacheKey = name
    if not wallObjectCache[cacheKey] and not wallCacheBuilding then
        wallCacheBuilding = true
        wallObjectCache[cacheKey] = {}
        -- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
        task.defer(function()
            local count = 0
            for _, obj in pairs(WS:GetDescendants()) do
                if obj:IsA("BasePart") and obj.Name == name then
                    table.insert(wallObjectCache[cacheKey], obj)
                    count = count + 1
                    if count % 50 == 0 then task.wait() end -- Invalid code Invalid code Invalid code
                end
            end
            wallCacheBuilding = false
        end)
        return nil -- Invalid code nil Invalid code Invalid code Invalid code
    end
    
    if not wallObjectCache[cacheKey] then return nil end
    
    local closest = nil
    local closestDist = HUGE
    for _, obj in pairs(wallObjectCache[cacheKey]) do
        if obj.Parent then
            local dist = (obj.Position - targetPos).Magnitude
            if dist < closestDist then
                closestDist = dist
                closest = obj
            end
        end
    end
    if closest and closestDist < 5 then return closest end
    return nil
end

local function enableWallbangHelper()
    getgenv().wallbangCache = {}
    -- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
    task.defer(function()
        for i, data in pairs(wallbangWalls) do
            local obj = findWallObject(data.name, data.pos)
            if obj then
                getgenv().wallbangCache[i] = {object = obj, original = obj.Transparency}
                obj.Transparency = 0.8
            end
            if i % 10 == 0 then task.wait() end -- Invalid code Invalid code Invalid code
        end
    end)
end

local function disableWallbangHelper()
    for _, cached in pairs(getgenv().wallbangCache) do
        if cached and cached.object then
            pcall(function() if cached.object.Parent then cached.object.Transparency = cached.original end end)
        end
    end
    getgenv().wallbangCache = {}
end

-- ========== REMOVE COLLISION FUNCTIONS ==========
local function enableRemoveCollision()
    getgenv().collisionCache = {}
    local folders = {
        WS:FindFirstChild("Map") and WS.Map:FindFirstChild("Clips"),
        WS:FindFirstChild("Map") and WS.Map:FindFirstChild("Doors"),
        WS:FindFirstChild("Map") and WS.Map:FindFirstChild("Ignore")
    }
    -- Invalid code Invalid code
    task.defer(function()
        local count = 0
        for _, folder in pairs(folders) do
            if folder then
                for _, obj in pairs(folder:GetDescendants()) do
                    if obj:IsA("BasePart") then
                        table.insert(getgenv().collisionCache, {object = obj, canCollide = obj.CanCollide, transparency = obj.Transparency})
                        obj.CanCollide = false
                        obj.Transparency = 1
                        count = count + 1
                        if count % 15 == 0 then task.wait() end -- Invalid code Invalid code Invalid code
                    end
                end
            end
        end
    end)
end

local function disableRemoveCollision()
    for _, cached in pairs(getgenv().collisionCache) do
        if cached and cached.object then
            pcall(function() if cached.object.Parent then cached.object.CanCollide = cached.canCollide cached.object.Transparency = cached.transparency end end)
        end
    end
    getgenv().collisionCache = {}
end

local function enableWallbangMap()
    getgenv().wallbangMapCache = {}
    local map = WS:FindFirstChild("Map")
    if not map then return end
    local geometry = map:FindFirstChild("Geometry")
    if not geometry then return end
    -- Invalid code Invalid code
    task.defer(function()
        local count = 0
        for _, obj in pairs(geometry:GetDescendants()) do
            if obj:IsA("BasePart") then
                table.insert(getgenv().wallbangMapCache, {object = obj, original = obj.Transparency})
                obj.Transparency = 0.5
                count = count + 1
                if count % 20 == 0 then task.wait() end -- Invalid code Invalid code Invalid code
            end
        end
    end)
end

local function disableWallbangMap()
    for _, cached in pairs(getgenv().wallbangMapCache) do
        if cached and cached.object then
            pcall(function() if cached.object.Parent then cached.object.Transparency = cached.original end end)
        end
    end
    getgenv().wallbangMapCache = {}
end


-- ========== HIT AIR SYSTEM (Invalid code Invalid code) ==========
local function HitAir_Create()
    if R.hitAirPart then return end
    if not R.myHRP then return end
    local part = Instance.new("Part")
    part.Name = "HitAirPlatform"
    part.Size = V3_NEW(4, 0.5, 4)
    part.Anchored = true
    part.CanCollide = true
    part.Transparency = 1
    part.CFrame = R.myHRP.CFrame * CFrame.new(0, -3, 0)
    part.Parent = WS
    R.hitAirPart = part
    R.hitAirActive = true
end

local function HitAir_Remove()
    if R.hitAirPart then
        pcall(function() R.hitAirPart:Destroy() end)
        R.hitAirPart = nil
    end
    R.hitAirActive = false
end

local function HitAir_Update()
    if not R.hitAirActive or not R.hitAirPart then return end
    if R.myHRP then
        R.hitAirPart.CFrame = CFrame.new(R.myHRP.Position - V3_NEW(0, 3, 0))
    end
end

-- ========== ENEMY AIR PLATFORM (Invalid code Air Shot Invalid code Invalid code) ==========
local enemyAirParts = {}

local function EnemyAir_Create(enemyHRP)
    if not enemyHRP then return nil end
    local part = Instance.new("Part")
    part.Name = "EnemyAirPlatform"
    part.Size = V3_NEW(6, 0.5, 6)
    part.Anchored = true
    part.CanCollide = true
    part.Transparency = 1
    part.CFrame = CFrame.new(enemyHRP.Position - V3_NEW(0, 3, 0))
    part.Parent = WS
    table.insert(enemyAirParts, part)
    -- Invalid code Invalid code 0.5 Invalid code
    task.delay(0.5, function()
        pcall(function() part:Destroy() end)
        for i, p in ipairs(enemyAirParts) do
            if p == part then table.remove(enemyAirParts, i) break end
        end
    end)
    return part
end

-- ========== HITMARKER SYSTEM ==========
local lastHitmarkerTime = 0
local HITMARKER_COOLDOWN = 1.5

local function ShowHitmarker(position, text)
    if not S.hmEnabled then return end
    local now = TICK()
    if now - lastHitmarkerTime < HITMARKER_COOLDOWN then return end
    lastHitmarkerTime = now
    
    local billboard = Instance.new("BillboardGui")
    billboard.Name = "Hitmarker"
    billboard.Size = UDim2.new(0, 300, 0, 80)
    billboard.StudsOffset = V3_NEW(0, 5, 0)
    billboard.AlwaysOnTop = true
    billboard.Adornee = nil
    
    local part = Instance.new("Part")
    part.Anchored = true
    part.CanCollide = false
    part.Transparency = 1
    part.Size = V3_NEW(0.1, 0.1, 0.1)
    part.Position = position + V3_NEW(0, 3, 0)
    part.Parent = WS
    billboard.Adornee = part
    billboard.Parent = part
    
    -- Invalid code Invalid code RESOLVED
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 1, 0)
    label.BackgroundTransparency = 1
    label.Text = text or "RESOLVED"
    label.TextColor3 = S.hmColor
    label.TextStrokeColor3 = Color3.new(0, 0, 0)
    label.TextStrokeTransparency = 0
    label.Font = Enum.Font.GothamBlack
    label.TextSize = 36
    label.TextScaled = false
    label.Parent = billboard
    
    -- Glow Invalid code Invalid code Invalid code
    local glow = Instance.new("TextLabel")
    glow.Size = UDim2.new(1, 0, 1, 0)
    glow.Position = UDim2.new(0, 2, 0, 2)
    glow.BackgroundTransparency = 1
    glow.Text = text or "RESOLVED"
    glow.TextColor3 = S.hmColor
    glow.TextTransparency = 0.5
    glow.TextStrokeTransparency = 1
    glow.Font = Enum.Font.GothamBlack
    glow.TextSize = 40
    glow.ZIndex = 0
    glow.Parent = billboard
    
    -- Invalid code - Invalid code Invalid code
    task.spawn(function()
        -- Invalid code zoom Invalid code
        label.TextSize = 10
        glow.TextSize = 14
        for i = 1, 8 do
            task.wait(0.015)
            local scale = 10 + i * 4
            label.TextSize = scale
            glow.TextSize = scale + 4
        end
        
        -- Invalid code
        for pulse = 1, 2 do
            for i = 1, 5 do
                task.wait(0.02)
                label.TextSize = 42 + i
                glow.TextSize = 46 + i
            end
            for i = 5, 1, -1 do
                task.wait(0.02)
                label.TextSize = 42 + i
                glow.TextSize = 46 + i
            end
        end
        
        task.wait(0.3)
        
        -- Invalid code Invalid code Invalid code
        for i = 1, 20 do
            task.wait(0.03)
            part.Position = part.Position + V3_NEW(0, 0.12, 0)
            local fade = i / 20
            label.TextTransparency = fade
            label.TextStrokeTransparency = fade
            glow.TextTransparency = 0.5 + fade * 0.5
        end
        part:Destroy()
    end)
end

-- ========== CUSTOM HIT SOUND SYSTEM ==========
local HitSounds = {
    ["Default"] = "139894735376184",
    ["Clash Royale Laugh"] = "8406005582",
    ["Satisfying Bell"] = "82635963679205",
    ["Poppy Pop"] = "5276754334",
    ["Sonic Rings"] = "1053865439",
    ["SLAP OH"] = "8907573127",
    ["Pixel Sonic Hit"] = "18350903398",
    ["Metal Pipe"] = "6729922069",
    ["Echo Fart"] = "7738058460",
    ["Victory Roblox"] = "12222253",
    ["Samsung Notif"] = "6879335951",
    ["Bruh"] = "6700501985",
    ["Library of Ruina"] = "9122014086",
    ["Fortnite Revive"] = "2128641323",
    ["Undertale"] = "3015952873",
    ["Lick"] = "1162994853",
    ["Light Punch"] = "3041190784",
    ["Medium Punch"] = "138285836",
    ["SUPER Punch"] = "6324690450",
    ["E"] = "2168246346",
    ["Discord"] = "5453349528",
    ["Minecraft Hit"] = "8766809464",
    ["Anime"] = "4936708385",
    ["Quack"] = "2101511513",
    ["Dark Souls"] = "8132494511",
    ["FNAF Bite"] = "8691323718",
    ["Scream"] = "7660049822",
    ["Meme Thud"] = "7149429798",
    ["Munch"] = "5630037419",
    ["Custom"] = ""
}
local HitSoundsList = {
    "Default", "Clash Royale Laugh", "Satisfying Bell", "Poppy Pop", "Sonic Rings", 
    "SLAP OH", "Pixel Sonic Hit", "Metal Pipe", "Echo Fart", "Victory Roblox",
    "Samsung Notif", "Bruh", "Library of Ruina", "Fortnite Revive", "Undertale",
    "Lick", "Light Punch", "Medium Punch", "SUPER Punch", "E",
    "Discord", "Minecraft Hit", "Anime", "Quack", "Dark Souls",
    "FNAF Bite", "Scream", "Meme Thud", "Munch", "Custom"
}

local lastHitSoundTime = 0
local HIT_SOUND_COOLDOWN = 1.5 -- 1500Invalid code Invalid code

local function PlayHitSound()
    if not S.hsEnabled then return end
    local now = TICK()
    if now - lastHitSoundTime < HIT_SOUND_COOLDOWN then return end
    lastHitSoundTime = now
    
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://" .. S.hsSoundId
    sound.Volume = (S.hsVolume / 100) * 3
    sound.Parent = game:GetService("SoundService")
    sound:Play()
    task.delay(4, function()
        pcall(function() sound:Destroy() end)
    end)
end

-- ========== KILL EFFECT SYSTEM ==========
local lastKillEffectTime = 0
local KILL_EFFECT_COOLDOWN = 1.5

local function ShowKillEffect(position)
    if not S.keEnabled then return end
    local now = TICK()
    if now - lastKillEffectTime < KILL_EFFECT_COOLDOWN then return end
    lastKillEffectTime = now
    
    -- Invalid code GLOW Invalid code
    local glowBall = Instance.new("Part")
    glowBall.Name = "KillGlowBall"
    glowBall.Shape = Enum.PartType.Ball
    glowBall.Size = V3_NEW(0.5, 0.5, 0.5)
    glowBall.Position = position
    glowBall.Anchored = true
    glowBall.CanCollide = false
    glowBall.Material = Enum.Material.Neon
    glowBall.Color = S.keColor
    glowBall.Transparency = 0
    glowBall.Parent = WS
    
    -- Invalid code Invalid code
    local mainLight = Instance.new("PointLight")
    mainLight.Color = S.keColor
    mainLight.Brightness = 10
    mainLight.Range = 25
    mainLight.Parent = glowBall
    
    -- Invalid code Invalid code (Invalid code)
    local core = Instance.new("Part")
    core.Name = "KillCore"
    core.Shape = Enum.PartType.Ball
    core.Size = V3_NEW(0.3, 0.3, 0.3)
    core.Position = position
    core.Anchored = true
    core.CanCollide = false
    core.Material = Enum.Material.Neon
    core.Color = Color3.new(1, 1, 1)
    core.Transparency = 0
    core.Parent = WS
    
    -- Invalid code Invalid code (Invalid code Invalid code 12 Invalid code 6)
    local particles = {}
    for i = 1, 6 do
        local p = Instance.new("Part")
        p.Name = "KillParticle"
        p.Shape = Enum.PartType.Ball
        p.Size = V3_NEW(0.2, 0.2, 0.2)
        p.Position = position
        p.Anchored = true
        p.CanCollide = false
        p.Material = Enum.Material.Neon
        p.Color = S.keColor
        p.Transparency = 0
        p.Parent = WS
        
        local theta = RAD(i * 60)
        local phi = RAD(math.random(-60, 60))
        local dir = V3_NEW(COS(theta) * COS(phi), SIN(phi) + 0.3, SIN(theta) * COS(phi))
        particles[i] = {part = p, dir = dir, speed = 0.8 + math.random() * 0.6}
    end
    
    -- Invalid code Invalid code Invalid code Invalid code
    local ring = Instance.new("Part")
    ring.Name = "KillRing"
    ring.Size = V3_NEW(0.3, 0.05, 0.3)
    ring.Position = position
    ring.Anchored = true
    ring.CanCollide = false
    ring.Material = Enum.Material.Neon
    ring.Color = S.keColor
    ring.Transparency = 0
    ring.Parent = WS
    
    -- Invalid code Invalid code (Invalid code Invalid code 4 Invalid code 2)
    local rays = {}
    for i = 1, 2 do
        local ray = Instance.new("Part")
        ray.Name = "KillRay"
        ray.Size = V3_NEW(0.1, 0.1, 3)
        ray.Position = position
        ray.Anchored = true
        ray.CanCollide = false
        ray.Material = Enum.Material.Neon
        ray.Color = S.keColor
        ray.Transparency = 0.3
        ray.Parent = WS
        
        local angle = RAD((i - 1) * 180)
        ray.CFrame = CFrame.new(position) * CFrame.Angles(0, angle, RAD(45))
        rays[i] = ray
    end
    
    -- Invalid code Invalid code Invalid code (Invalid code - Invalid code Invalid code)
    task.spawn(function()
        -- Invalid code 1: Invalid code Invalid code Invalid code (Invalid code Invalid code 10 Invalid code 6 Invalid code)
        for i = 1, 6 do
            task.wait(0.02)
            local t = i / 6
            local size = 0.5 + t * 3
            glowBall.Size = V3_NEW(size, size, size)
            core.Size = V3_NEW(size * 0.6, size * 0.6, size * 0.6)
            mainLight.Brightness = 10 + t * 15
            mainLight.Range = 25 + t * 20
        end
        
        -- Invalid code 2: Invalid code! (Invalid code Invalid code 20 Invalid code 12 Invalid code)
        for i = 1, 12 do
            task.wait(0.035)
            local t = i / 12
            
            -- Invalid code Invalid code
            local ballSize = 3.5 * (1 - t * 0.8)
            glowBall.Size = V3_NEW(ballSize, ballSize, ballSize)
            glowBall.Transparency = t * 0.9
            core.Size = V3_NEW(ballSize * 0.5, ballSize * 0.5, ballSize * 0.5)
            core.Transparency = t
            mainLight.Brightness = 25 * (1 - t)
            
            -- Invalid code Invalid code
            for _, data in ipairs(particles) do
                local dist = t * 12 * data.speed
                data.part.Position = position + data.dir * dist
                local pSize = 0.2 * (1 - t * 0.7)
                data.part.Size = V3_NEW(pSize, pSize, pSize)
                data.part.Transparency = t * 0.9
            end
            
            -- Invalid code Invalid code
            local ringSize = 0.3 + t * 15
            ring.Size = V3_NEW(ringSize, 0.05, ringSize)
            ring.Transparency = t * 0.95
            
            -- Invalid code Invalid code Invalid code Invalid code
            for _, ray in ipairs(rays) do
                local rayLen = 3 + t * 8
                ray.Size = V3_NEW(0.1 * (1 - t * 0.5), 0.1 * (1 - t * 0.5), rayLen)
                ray.Transparency = 0.3 + t * 0.7
            end
        end
        
        -- Cleanup
        glowBall:Destroy()
        core:Destroy()
        for _, data in ipairs(particles) do
            data.part:Destroy()
        end
        ring:Destroy()
        for _, ray in ipairs(rays) do
            ray:Destroy()
        end
    end)
end

-- ========== FORTNITE DAMAGE SYSTEM ==========
local function ShowFortniteDamage(position, damage)
    if not S.fdmgEnabled then return end
    
    local billboard = Instance.new("BillboardGui")
    billboard.Name = "FortniteDmg"
    billboard.Size = UDim2.new(0, 100, 0, 40)
    billboard.StudsOffset = V3_NEW(math.random(-10, 10) / 10, 2, math.random(-10, 10) / 10)
    billboard.AlwaysOnTop = true
    
    local part = Instance.new("Part")
    part.Anchored = true
    part.CanCollide = false
    part.Transparency = 1
    part.Size = V3_NEW(0.1, 0.1, 0.1)
    part.Position = position + V3_NEW(0, 1.5, 0)
    part.Parent = WS
    billboard.Adornee = part
    billboard.Parent = part
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 1, 0)
    label.BackgroundTransparency = 1
    label.Text = tostring(damage)
    label.TextColor3 = S.fdmgColor
    label.TextStrokeColor3 = Color3.new(0, 0, 0)
    label.TextStrokeTransparency = 0
    label.Font = Enum.Font.GothamBlack
    label.TextSize = 28
    label.TextScaled = false
    label.Parent = billboard
    
    -- Invalid code Invalid code Invalid code Fortnite - Invalid code Invalid code Invalid code
    task.spawn(function()
        local startY = part.Position.Y
        for i = 1, 20 do
            task.wait(0.035)
            local t = i / 20
            -- Invalid code Invalid code Invalid code Invalid code
            part.Position = V3_NEW(part.Position.X, startY + t * 2.5, part.Position.Z)
            -- Invalid code Invalid code Invalid code Invalid code Invalid code
            local scale = 1 + math.sin(t * 3.14) * 0.3
            label.TextSize = 28 * scale
            -- Invalid code Invalid code Invalid code
            if t > 0.6 then
                local fadeT = (t - 0.6) / 0.4
                label.TextTransparency = fadeT
                label.TextStrokeTransparency = fadeT
            end
        end
        part:Destroy()
    end)
end

-- ========== DEBUG CONSOLE SYSTEM ==========
local function DC_CreateUI()
    if R.dcFrame then pcall(function() R.dcFrame:Destroy() end) end
    
    local frame = Instance.new("Frame")
    frame.Name = "DebugConsole"
    frame.Size = UDim2.new(0, 300, 0, 200)
    frame.Position = UDim2.new(0, 10, 0.5, -100)
    frame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
    frame.BackgroundTransparency = 0.05
    frame.BorderSizePixel = 0
    frame.Visible = S.dcVisible
    frame.Active = true
    frame.Parent = R.gui
    R.dcFrame = frame
    
    local stroke = Instance.new("UIStroke", frame)
    stroke.Color = Color3.fromRGB(40, 40, 40)
    stroke.Thickness = 1
    
    -- Title bar
    local titleBar = Instance.new("Frame")
    titleBar.Size = UDim2.new(1, 0, 0, 18)
    titleBar.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
    titleBar.BorderSizePixel = 0
    titleBar.Parent = frame
    
    local title = Instance.new("TextLabel")
    title.Text = "console"
    title.Size = UDim2.new(1, -5, 1, 0)
    title.Position = UDim2.new(0, 5, 0, 0)
    title.BackgroundTransparency = 1
    title.Font = T.Font
    title.TextSize = 10
    title.TextColor3 = Color3.fromRGB(100, 100, 100)
    title.TextXAlignment = Enum.TextXAlignment.Left
    title.Parent = titleBar
    
    -- Scroll frame for logs
    local scroll = Instance.new("ScrollingFrame")
    scroll.Name = "Logs"
    scroll.Size = UDim2.new(1, -6, 1, -22)
    scroll.Position = UDim2.new(0, 3, 0, 20)
    scroll.BackgroundTransparency = 1
    scroll.BorderSizePixel = 0
    scroll.ScrollBarThickness = 3
    scroll.ScrollBarImageColor3 = Color3.fromRGB(60, 60, 60)
    scroll.ScrollingDirection = Enum.ScrollingDirection.Y
    scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
    scroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
    scroll.Parent = frame
    
    local layout = Instance.new("UIListLayout")
    layout.Padding = UDim.new(0, 1)
    layout.SortOrder = Enum.SortOrder.LayoutOrder
    layout.Parent = scroll
    
    -- Drag
    local drag, dragStart, dragPos = false, nil, nil
    titleBar.InputBegan:Connect(function(i)
        if i.UserInputType == Enum.UserInputType.MouseButton1 then
            drag, dragStart, dragPos = true, i.Position, frame.Position
        end
    end)
    titleBar.InputChanged:Connect(function(i)
        if i.UserInputType == Enum.UserInputType.MouseMovement and drag then
            local d = i.Position - dragStart
            frame.Position = UDim2.new(dragPos.X.Scale, dragPos.X.Offset + d.X, dragPos.Y.Scale, dragPos.Y.Offset + d.Y)
        end
    end)
    table.insert(R.conns, UIS.InputEnded:Connect(function(i)
        if i.UserInputType == Enum.UserInputType.MouseButton1 then drag = false end
    end))
end

local function DC_Log(text, logType)
    if not S.dcEnabled then return end
    if not R.dcFrame then DC_CreateUI() end
    
    local scroll = R.dcFrame and R.dcFrame:FindFirstChild("Logs")
    if not scroll then return end
    
    -- Invalid code Invalid code Invalid code
    local colors = {
        kill = Color3.fromRGB(255, 80, 80),
        damage = Color3.fromRGB(255, 200, 100),
        headshot = Color3.fromRGB(255, 120, 120),
        bodyshot = Color3.fromRGB(255, 180, 100),
        legshot = Color3.fromRGB(200, 160, 100),
        miss = Color3.fromRGB(120, 120, 120),
        info = Color3.fromRGB(150, 200, 255)
    }
    
    local entry = Instance.new("TextLabel")
    entry.Size = UDim2.new(1, -5, 0, 12)
    entry.BackgroundTransparency = 1
    entry.Font = T.Font
    entry.TextSize = 10
    entry.TextColor3 = colors[logType] or Color3.fromRGB(180, 180, 180)
    entry.TextXAlignment = Enum.TextXAlignment.Left
    entry.Text = os.date("%H:%M:%S") .. " " .. text
    entry.LayoutOrder = #R.dcLogs + 1
    entry.Parent = scroll
    
    table.insert(R.dcLogs, entry)
    
    -- Invalid code Invalid code
    if #R.dcLogs > R.dcMaxLogs then
        local old = table.remove(R.dcLogs, 1)
        if old then pcall(function() old:Destroy() end) end
    end
    
    -- Invalid code-Invalid code Invalid code
    scroll.CanvasPosition = Vector2.new(0, scroll.AbsoluteCanvasSize.Y)
    
    -- Notify Invalid code Invalid code
    if S.dcNotify then
        DC_ShowNotify(text, logType)
    end
end

local function DC_ShowNotify(text, logType)
    if not R.gui then return end
    if not S.dcNotify then return end
    
    local colors = {
        kill = Color3.fromRGB(255, 80, 80),
        damage = Color3.fromRGB(255, 200, 100),
        headshot = Color3.fromRGB(255, 120, 120),
        miss = Color3.fromRGB(150, 150, 150),
        info = Color3.fromRGB(150, 200, 255)
    }
    
    R.dcNotifies = R.dcNotifies or {}
    
    -- Invalid code Invalid code Invalid code Invalid code
    for _, n in ipairs(R.dcNotifies) do
        if n and n.Parent then
            n.Position = UDim2.new(n.Position.X.Scale, n.Position.X.Offset, n.Position.Y.Scale, n.Position.Y.Offset - 28)
        end
    end
    
    local notify = Instance.new("Frame")
    notify.Size = UDim2.new(0, 280, 0, 24)
    notify.Position = UDim2.new(0.5, -140, 1, -60)
    notify.BackgroundColor3 = Color3.fromRGB(12, 12, 12)
    notify.BackgroundTransparency = 0.15
    notify.BorderSizePixel = 0
    notify.Parent = R.gui
    
    Instance.new("UIStroke", notify).Color = Color3.fromRGB(35, 35, 35)
    Instance.new("UICorner", notify).CornerRadius = UDim.new(0, 4)
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -10, 1, 0)
    label.Position = UDim2.new(0, 5, 0, 0)
    label.BackgroundTransparency = 1
    label.Font = T.Font
    label.TextSize = 12
    label.TextColor3 = colors[logType] or Color3.fromRGB(180, 180, 180)
    label.Text = text
    label.TextXAlignment = Enum.TextXAlignment.Center
    label.Parent = notify
    
    table.insert(R.dcNotifies, notify)
    
    -- Invalid code Invalid code
    if #R.dcNotifies > 5 then
        local old = table.remove(R.dcNotifies, 1)
        if old then pcall(function() old:Destroy() end) end
    end
    
    task.delay(2.5, function()
        for i, n in ipairs(R.dcNotifies) do
            if n == notify then
                table.remove(R.dcNotifies, i)
                break
            end
        end
        for i = 1, 10 do
            task.wait(0.02)
            notify.BackgroundTransparency = 0.15 + i * 0.085
            label.TextTransparency = i * 0.1
        end
        notify:Destroy()
    end)
end

local function DC_Toggle()
    S.dcVisible = not S.dcVisible
    if not R.dcFrame then DC_CreateUI() end
    if R.dcFrame then R.dcFrame.Visible = S.dcVisible end
    
    if S.dcVisible then
        R.hotkeys["Console"] = {active = true, key = S.dcKey.Name}
    else
        R.hotkeys["Console"] = nil
    end
    UpdateHotkeyList()
end

-- ========== HIT LOGGER (Console Style) ==========
CreateHitLogger = function()
    if R.hlFrame then pcall(function() R.hlFrame:Destroy() end) end
    if not R.gui then return end
    
    -- Invalid code Invalid code Invalid code Invalid code, Invalid code Invalid code (65%)
    local f = Instance.new("Frame", R.gui)
    f.Name = "HitLogger"
    f.Size = UDim2.new(0, 550, 0, 200)
    f.Position = UDim2.new(0.5, -275, 0.65, 0)
    f.BackgroundTransparency = 1
    f.BorderSizePixel = 0
    f.Visible = S.hlEnabled
    
    R.hlFrame = f
end

AddHitLog = function(logType, playerName, hitbox, damage)
    if not S.hlEnabled then return end
    if not R.hlFrame then CreateHitLogger() end
    if not R.hlFrame then return end
    
    -- Invalid code Invalid code Invalid code Invalid code Invalid code
    local targetPos = nil
    for _, plr in ipairs(Plrs:GetPlayers()) do
        if plr.Name == playerName and plr.Character then
            local hrp = plr.Character:FindFirstChild("HumanoidRootPart")
            if hrp then targetPos = hrp.Position end
            break
        end
    end
    
    -- Invalid code Invalid code Invalid code (Invalid code Invalid code)
    if logType == "kill" and targetPos then
        PlayHitSound()
        ShowHitmarker(targetPos, "RESOLVED")
        ShowKillEffect(targetPos)
    end
    
    -- Fortnite Damage Invalid code Invalid code
    if logType == "hit" and targetPos and damage and damage > 0 then
        ShowFortniteDamage(targetPos, damage)
    end
    
    local dmg = logType == "kill" and 100 or (damage or 0)
    local remaining = logType == "kill" and 0 or (logType == "miss" and 100 or (100 - dmg))
    if remaining < 0 then remaining = 0 end
    if remaining < 0 then remaining = 0 end
    
    R.hlLogIndex = (R.hlLogIndex or 0) + 1
    local myIndex = R.hlLogIndex
    
    -- Invalid code Invalid code Invalid code Invalid code
    local TweenS = game:GetService("TweenService")
    for _, child in ipairs(R.hlFrame:GetChildren()) do
        if child:IsA("Frame") and child.Name:match("^Log_") then
            local currentY = child.Position.Y.Offset
            TweenS:Create(child, TweenInfo.new(0.15, Enum.EasingStyle.Quad), {
                Position = UDim2.new(0.5, 0, 0, currentY + 24)
            }):Play()
        end
    end
    
    -- Invalid code Invalid code
    local actionText = logType == "kill" and "Killed" or (logType == "miss" and "Missed" or "Hurt")
    local hitboxLower = (hitbox or "head"):lower()
    local fullText
    if logType == "miss" then
        fullText = string.format("✕ Missed %s (shot at %s).", playerName or "Unknown", hitboxLower)
    else
        fullText = string.format("%s %s in the %s for %d hp (%d remaining).", actionText, playerName or "Unknown", hitboxLower, dmg, remaining)
    end
    
    -- Invalid code - Invalid code Invalid code
    local bar = Instance.new("Frame", R.hlFrame)
    bar.Name = "Log_" .. myIndex
    bar.Size = UDim2.new(0, 480, 0, 24)
    bar.Position = UDim2.new(0.5, 0, 0, 0)
    bar.AnchorPoint = Vector2.new(0.5, 0)
    bar.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
    bar.BackgroundTransparency = 0.5
    bar.BorderSizePixel = 0
    bar.ZIndex = 100
    
    Instance.new("UICorner", bar).CornerRadius = UDim.new(0, 4)
    
    -- RGB Invalid code Invalid code (Invalid code)
    local rgbLine = Instance.new("Frame", bar)
    rgbLine.Name = "RGBLine"
    rgbLine.Size = UDim2.new(1, 0, 0, 2)
    rgbLine.Position = UDim2.new(0, 0, 0, 0)
    rgbLine.BackgroundColor3 = Color3.new(1, 1, 1)
    rgbLine.BackgroundTransparency = 0
    rgbLine.BorderSizePixel = 0
    rgbLine.ZIndex = 102
    
    -- RGB Invalid code
    local rgbGradient = Instance.new("UIGradient", rgbLine)
    rgbGradient.Color = ColorSequence.new({
        ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 0)),
        ColorSequenceKeypoint.new(0.17, Color3.fromRGB(255, 255, 0)),
        ColorSequenceKeypoint.new(0.33, Color3.fromRGB(0, 255, 0)),
        ColorSequenceKeypoint.new(0.5, Color3.fromRGB(0, 255, 255)),
        ColorSequenceKeypoint.new(0.67, Color3.fromRGB(0, 0, 255)),
        ColorSequenceKeypoint.new(0.83, Color3.fromRGB(255, 0, 255)),
        ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 0, 0))
    })
    
    -- Invalid code RGB Invalid code (Invalid code - Invalid code Invalid code Invalid code)
    local startTime = TICK()
    local rgbConn
    rgbConn = RS.Heartbeat:Connect(function()
        if not rgbLine or not rgbLine.Parent then
            if rgbConn then rgbConn:Disconnect() end
            return
        end
        local offset = ((TICK() - startTime) * 0.4) % 1
        rgbGradient.Offset = Vector2.new(offset, 0)
    end)
    
    -- Invalid code Invalid code Invalid code
    bar.Destroying:Connect(function()
        if rgbConn then rgbConn:Disconnect() end
    end)
    
    Instance.new("UICorner", rgbLine).CornerRadius = UDim.new(0, 1)
    
    -- Invalid code (miss = X Invalid code, hit/kill = + Invalid code)
    local iconLabel = Instance.new("TextLabel", bar)
    iconLabel.Size = UDim2.new(0, 24, 1, 0)
    iconLabel.Position = UDim2.new(0, 8, 0, 0)
    iconLabel.BackgroundTransparency = 1
    iconLabel.Text = logType == "miss" and "X" or "+"
    iconLabel.Font = Enum.Font.GothamBold
    iconLabel.TextSize = 14
    iconLabel.TextColor3 = logType == "miss" and Color3.fromRGB(255, 80, 80) or Color3.fromRGB(100, 255, 100)
    iconLabel.ZIndex = 102
    
    -- Invalid code (Invalid code Invalid code)
    local textLabel = Instance.new("TextLabel", bar)
    textLabel.Size = UDim2.new(1, -38, 1, 0)
    textLabel.Position = UDim2.new(0, 30, 0, 0)
    textLabel.BackgroundTransparency = 1
    textLabel.Text = fullText
    textLabel.Font = Enum.Font.Code
    textLabel.TextSize = 13
    textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    textLabel.TextXAlignment = Enum.TextXAlignment.Left
    textLabel.TextTruncate = Enum.TextTruncate.AtEnd
    textLabel.ZIndex = 101
    
    -- Invalid code Invalid code
    bar.Position = UDim2.new(0.5, 0, 0, -20)
    bar.BackgroundTransparency = 1
    textLabel.TextTransparency = 1
    iconLabel.TextTransparency = 1
    rgbLine.BackgroundTransparency = 1
    
    local slideInfo = TweenInfo.new(0.2, Enum.EasingStyle.Quint, Enum.EasingDirection.Out)
    local fadeInfo = TweenInfo.new(0.15)
    
    TweenS:Create(bar, slideInfo, {Position = UDim2.new(0.5, 0, 0, 0)}):Play()
    TweenS:Create(bar, fadeInfo, {BackgroundTransparency = 0.5}):Play()
    TweenS:Create(textLabel, fadeInfo, {TextTransparency = 0}):Play()
    TweenS:Create(iconLabel, fadeInfo, {TextTransparency = 0}):Play()
    TweenS:Create(rgbLine, fadeInfo, {BackgroundTransparency = 0}):Play()
    
    -- Invalid code Invalid code Invalid code
    local logs = {}
    for _, child in ipairs(R.hlFrame:GetChildren()) do
        if child:IsA("Frame") and child.Name:match("^Log_") then
            table.insert(logs, child)
        end
    end
    
    table.sort(logs, function(a, b)
        local numA = tonumber(a.Name:match("Log_(%d+)")) or 0
        local numB = tonumber(b.Name:match("Log_(%d+)")) or 0
        return numA < numB
    end)
    
    while #logs > S.hlMaxLogs do
        local oldest = logs[1]
        if oldest then
            oldest:Destroy()
            table.remove(logs, 1)
        end
    end
    
    -- Invalid code Invalid code 5 Invalid code
    task.delay(5, function()
        if bar and bar.Parent then
            local TweenService = game:GetService("TweenService")
            local fadeOut = TweenInfo.new(0.3, Enum.EasingStyle.Quad)
            
            TweenService:Create(bar, fadeOut, {BackgroundTransparency = 1}):Play()
            TweenService:Create(textLabel, fadeOut, {TextTransparency = 1}):Play()
            TweenService:Create(iconLabel, fadeOut, {TextTransparency = 1}):Play()
            TweenService:Create(rgbLine, fadeOut, {BackgroundTransparency = 1}):Play()
            
            task.delay(0.35, function()
                if bar and bar.Parent then bar:Destroy() end
            end)
        end
    end)
end

-- ========== KILL TRACKING ==========
local trackedEnemies = {}
local lastShotTarget = nil
local lastShotHitbox = nil
local lastShotTime = 0
local lastKillTime = 0
local lastHitTime = 0
local lastMissTime = 0
local pendingShots = {}
local lastShotByTarget = {}
local confirmedShots = {} -- Invalid code Invalid code

TrackShot = function(playerName, hitbox)
    if not playerName or playerName == "" then return end
    
    -- Invalid code Invalid code Invalid code Invalid code
    local targetPlayer = nil
    for _, p in ipairs(Plrs:GetPlayers()) do
        if p.Name == playerName then
            targetPlayer = p
            break
        end
    end
    if not targetPlayer or targetPlayer == LP then return end
    
    -- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
    local targetChar = targetPlayer.Character
    if not targetChar then return end
    
    local targetHum = targetChar:FindFirstChildOfClass("Humanoid")
    if not targetHum or targetHum.Health <= 0 then return end
    
    local now = TICK()
    lastShotTarget = playerName
    lastShotHitbox = hitbox or "Head"
    lastShotTime = now
    
    -- Invalid code Invalid code pending shot Invalid code Invalid code Invalid code
    if lastShotByTarget[playerName] then
        pendingShots[lastShotByTarget[playerName]] = nil
    end
    
    local shotId = now .. "_" .. math.random(1000, 9999)
    pendingShots[shotId] = {target = playerName, hitbox = hitbox or "Head", time = now}
    lastShotByTarget[playerName] = shotId
    
    -- Invalid code Invalid code Invalid code Invalid code (Invalid code 1 Invalid code)
    confirmedShots[playerName] = now
    
    task.delay(1.2, function()
        if pendingShots[shotId] then
            local checkTime = TICK()
            -- Invalid code Invalid code miss
            if checkTime - lastMissTime > 0.5 and checkTime - lastHitTime > 0.5 and checkTime - lastKillTime > 0.5 then
                -- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
                local p = Plrs:FindFirstChild(playerName)
                if p and p.Character then
                    local h = p.Character:FindFirstChildOfClass("Humanoid")
                    if h and h.Health > 0 then
                        lastMissTime = checkTime
                        AddHitLog("miss", playerName, hitbox or "Head", 0)
                    end
                end
            end
            pendingShots[shotId] = nil
            if lastShotByTarget[playerName] == shotId then
                lastShotByTarget[playerName] = nil
            end
        end
    end)
    
    -- Invalid code Invalid code Invalid code Invalid code
    task.delay(1.5, function()
        if confirmedShots[playerName] == now then
            confirmedShots[playerName] = nil
        end
    end)
end

local function ResolveShots(playerName)
    for id, shot in pairs(pendingShots) do
        if shot.target == playerName then
            pendingShots[id] = nil
        end
    end
    lastShotByTarget[playerName] = nil
end

local function setupHumTracking(plr, hum)
    if not plr or not hum then return end
    if plr == LP then return end
    
    local lastHealth = hum.Health
    local lastDamageTime = 0
    
    hum.HealthChanged:Connect(function(newHealth)
        local now = TICK()
        local damage = FLOOR(lastHealth - newHealth)
        
        -- Invalid code Invalid code: Invalid code Invalid code Invalid code
        local myTeam = LP.Team
        local myColor = LP.TeamColor
        if myTeam and (plr.Team == myTeam or plr.TeamColor == myColor) then
            lastHealth = newHealth
            return
        end
        
        -- Invalid code Invalid code: Invalid code Invalid code Invalid code Invalid code Invalid code
        local isOurShot = false
        
        -- Invalid code 1: Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
        if confirmedShots[plr.Name] and now - confirmedShots[plr.Name] < 0.6 then
            isOurShot = true
        end
        
        -- Invalid code 2: lastShotTarget Invalid code Invalid code Invalid code Invalid code (Invalid code)
        if lastShotTarget == plr.Name and now - lastShotTime < 0.5 then
            isOurShot = true
        end
        
        if not isOurShot then
            lastHealth = newHealth
            return
        end
        
        -- Invalid code Invalid code Invalid code
        if now - lastDamageTime < 0.15 then
            lastHealth = newHealth
            return
        end
        lastDamageTime = now
        
        ResolveShots(plr.Name)
        
        if newHealth <= 0 and lastHealth > 0 then
            -- Kill - Invalid code Invalid code Invalid code (Invalid code)
            if now - lastKillTime > 0.5 and now - lastShotTime < 0.5 then
                lastKillTime = now
                confirmedShots[plr.Name] = nil -- Invalid code Invalid code kill
                AddHitLog("kill", plr.Name, lastShotHitbox or "Head", FLOOR(lastHealth))
            end
        elseif damage > 0 and damage < 150 then
            -- Hit (Invalid code Invalid code Invalid code Invalid code)
            if now - lastHitTime > 0.2 then
                lastHitTime = now
                AddHitLog("hit", plr.Name, lastShotHitbox or "Head", damage)
            end
        end
        
        lastHealth = newHealth
    end)
    
    hum.Died:Connect(function()
        task.delay(0.5, function()
            lastHealth = 100
        end)
    end)
end

SetupKillTracking = function()
    for _, player in ipairs(Plrs:GetPlayers()) do
        if player ~= LP and not trackedEnemies[player.UserId] then
            trackedEnemies[player.UserId] = true
            
            local function onCharacter(char)
                if not char then return end
                local hum = char:FindFirstChildOfClass("Humanoid")
                if hum then
                    setupHumTracking(player, hum)
                else
                    task.delay(0.5, function()
                        if char and char.Parent then
                            local h = char:FindFirstChildOfClass("Humanoid")
                            if h then
                                setupHumTracking(player, h)
                            end
                        end
                    end)
                end
            end
            
            if player.Character then onCharacter(player.Character) end
            player.CharacterAdded:Connect(onCharacter)
        end
    end
end

Plrs.PlayerAdded:Connect(function(player)
    if player ~= LP then
        task.delay(1, SetupKillTracking)
    end
end)

-- ========== ANIM BREAKER SYSTEM ==========
local function AB_Enable()
    if R.abActive then return end
    if not R.myChar or not R.myHum then return end
    
    R.abActive = true
    R.hotkeys["AnimBreak"] = {active = true, key = S.abKey.Name}
    UpdateHotkeyList()
    
    -- Invalid code Invalid code Invalid code
    local animator = R.myHum:FindFirstChildOfClass("Animator")
    if animator then
        for _, track in ipairs(animator:GetPlayingAnimationTracks()) do
            pcall(function() track:Stop() end)
        end
    end
    
    -- Invalid code Invalid code Invalid code
    R.abConn = RS.Heartbeat:Connect(function()
        if not R.abActive or not R.myHum then return end
        local anim = R.myHum:FindFirstChildOfClass("Animator")
        if anim then
            for _, track in ipairs(anim:GetPlayingAnimationTracks()) do
                pcall(function() track:Stop(0) end)
            end
        end
    end)
end

local function AB_Disable()
    if not R.abActive then return end
    
    if R.abConn then
        R.abConn:Disconnect()
        R.abConn = nil
    end
    
    R.abActive = false
    R.hotkeys["AnimBreak"] = nil
    UpdateHotkeyList()
end

-- ========== AI PEEK SYSTEM V4 ==========
local AP_SHOOT_DELAY = 0.05
local AP_GROUND_OFFSET = 2.8
local AP_RINGS = 2
local AP_POINTS_PER_RING = 8
local AP_RING_SPACING = 4

local apPoints = {}
local apESPCache = {}
local apLastUpdate = 0

local function AP_GetMoveParams(dist)
    local speed = S.apSpeed
    if speed <= 0 then return 0, 0, true end
    local baseTime
    if speed <= 50 then baseTime = 0.02 + (speed / 50) * 0.05
    elseif speed <= 200 then baseTime = 0.07 + ((speed - 50) / 150) * 0.1
    elseif speed <= 500 then baseTime = 0.17 + ((speed - 200) / 300) * 0.2
    elseif speed <= 800 then baseTime = 0.37 + ((speed - 500) / 300) * 0.25
    else baseTime = 0.62 + ((speed - 800) / 200) * 0.3 end
    local distFactor = CLAMP(dist / 15, 0.5, 2.0)
    local totalTime = baseTime * distFactor
    local steps = CLAMP(CEIL(totalTime * 60), 2, 50)
    return totalTime, steps, false
end

local function AP_SmoothStep(t) return t * t * (3 - 2 * t) end

local function AP_ValidateHeight(startY, endY)
    local maxHeight = S.apHeight
    local diff = endY - startY
    if diff > maxHeight then return startY + maxHeight end
    if diff < -maxHeight then return startY - maxHeight end
    return endY
end

local function AP_ClearESP()
    for _, hl in pairs(apESPCache) do pcall(function() hl:Destroy() end) end
    apESPCache = {}
end

local function AP_CanSee(from, to, ignore)
    APRayP.FilterDescendantsInstances = ignore
    local r = WS:Raycast(from, to - from, APRayP)
    return not r or not r.Instance.CanCollide
end

local function AP_GetGround(pos, char, baseY)
    APRayP.FilterDescendantsInstances = {char}
    local r = WS:Raycast(pos + V3_NEW(0, 6, 0), V3_NEW(0, -18, 0), APRayP)
    if r then
        local gy = r.Position.Y + 0.3
        return AP_ValidateHeight(baseY, gy), true
    end
    return baseY, false
end

local function AP_CanReach(from, to, char)
    APRayP.FilterDescendantsInstances = {char}
    for _, h in ipairs({1, 2, 3}) do
        local s, e = from + V3_NEW(0, h, 0), to + V3_NEW(0, h, 0)
        local r = WS:Raycast(s, e - s, APRayP)
        if r and r.Instance.CanCollide then return false end
    end
    return true
end

local function AP_EnemySeesPoint(enemyRoot, pointPos, myChar, enemyChar)
    APRayP.FilterDescendantsInstances = {myChar, enemyChar}
    local enemyEye = enemyRoot.Position + V3_NEW(0, 1.5, 0)
    for _, h in ipairs({1, 2.5}) do
        local target = pointPos + V3_NEW(0, h, 0)
        local result = WS:Raycast(enemyEye, target - enemyEye, APRayP)
        if not result or not result.Instance.CanCollide then return true end
    end
    return false
end

local function AP_CanShootFrom(pointPos, enemyHead, myChar, enemyChar)
    if not enemyHead then return false end
    return AP_CanSee(pointPos + V3_NEW(0, 1.6, 0), enemyHead.Position, {myChar, enemyChar})
end

local function AP_EnemyVisible(myRoot, myChar, enemyChar)
    local head = enemyChar:FindFirstChild("Head")
    local torso = enemyChar:FindFirstChild("UpperTorso") or enemyChar:FindFirstChild("Torso")
    local eye = myRoot.Position + V3_NEW(0, 1.6, 0)
    local ignore = {myChar, enemyChar}
    if head and AP_CanSee(eye, head.Position, ignore) then return true end
    if torso and AP_CanSee(eye, torso.Position, ignore) then return true end
    return false
end

local function AP_CanPeekPlayer(myRoot, myChar, targetChar)
    local targetHead = targetChar:FindFirstChild("Head")
    if not targetHead then return false end
    for i = 1, #apPoints do
        local pt = apPoints[i]
        if pt and pt.canReach then
            APRayP.FilterDescendantsInstances = {myChar, targetChar}
            local eyePos = pt.pos + V3_NEW(0, 1.6, 0)
            local r = WS:Raycast(eyePos, targetHead.Position - eyePos, APRayP)
            if not r or not r.Instance.CanCollide then return true end
        end
    end
    return false
end

local function AP_UpdateESP()
    if not S.apESP or not R.apActive then AP_ClearESP() return end
    local char = LP.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    local myTeam, myColor = LP.Team, LP.TeamColor
    local processed = {}
    for _, p in ipairs(Plrs:GetPlayers()) do
        if p == LP then continue end
        if S.apTeamCheck and myTeam and (p.Team == myTeam or p.TeamColor == myColor) then continue end
        local c = p.Character
        if not c then continue end
        local h = c:FindFirstChildOfClass("Humanoid")
        if not h or h.Health <= 0 then continue end
        processed[p.UserId] = true
        local canPeek = AP_CanPeekPlayer(root, char, c)
        local existing = apESPCache[p.UserId]
        if existing and existing.Parent then
            existing.OutlineColor = canPeek and Color3.fromRGB(50, 255, 50) or Color3.fromRGB(255, 50, 50)
            existing.FillColor = canPeek and Color3.fromRGB(50, 255, 50) or Color3.fromRGB(255, 50, 50)
        else
            local hl = Instance.new("Highlight")
            hl.Name = "AP_ESP"
            hl.Adornee = c
            hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
            hl.FillTransparency = 0.85
            hl.OutlineTransparency = 0
            hl.OutlineColor = canPeek and Color3.fromRGB(50, 255, 50) or Color3.fromRGB(255, 50, 50)
            hl.FillColor = canPeek and Color3.fromRGB(50, 255, 50) or Color3.fromRGB(255, 50, 50)
            hl.Parent = c
            apESPCache[p.UserId] = hl
        end
    end
    for id, hl in pairs(apESPCache) do
        if not processed[id] then pcall(function() hl:Destroy() end) apESPCache[id] = nil end
    end
end

local function AP_CreatePoints()
    for i = 1, #apPoints do
        local p = apPoints[i]
        if p and p.part then p.part:Destroy() end
        apPoints[i] = nil
    end
    local baseSpacing = S.apPeekDist / AP_RINGS
    local idx = 0
    for ring = 1, AP_RINGS do
        local dist = ring * baseSpacing * (AP_RING_SPACING / 4)
        for i = 1, AP_POINTS_PER_RING do
            idx = idx + 1
            local angle = (i - 1) * (360 / AP_POINTS_PER_RING) + (ring * 22.5)
            apPoints[idx] = {ring = ring, dist = dist, angle = angle, pos = V3_ZERO, groundY = 0, canReach = false, canShoot = false, enemySees = false, score = 0, part = nil}
            if S.apShowPoints then
                local p = Instance.new("Part")
                p.Name = "AP_P"
                p.Size = V3_NEW(0.5, 0.5, 0.5)
                p.Shape = Enum.PartType.Ball
                p.Anchored = true
                p.CanCollide = false
                p.CastShadow = false
                p.Material = Enum.Material.Neon
                p.Transparency = 0.3
                p.Color = Color3.new(1, 1, 1)
                p.Parent = WS
                apPoints[idx].part = p
            end
        end
    end
    R.apPoints = apPoints
    R.apPointCount = idx
end

local function AP_RemovePoints()
    for i = 1, #apPoints do
        local p = apPoints[i]
        if p and p.part then p.part:Destroy() end
        apPoints[i] = nil
    end
    R.apPoints = {}
    R.apPointCount = 0
    AP_ClearESP()
end

local function AP_UpdatePoints(rootPos, char, baseY)
    for i = 1, #apPoints do
        local pt = apPoints[i]
        if not pt then continue end
        local ang = RAD(pt.angle)
        local basePos = rootPos + V3_NEW(COS(ang) * pt.dist, 0, SIN(ang) * pt.dist)
        pt.groundY, _ = AP_GetGround(basePos, char, baseY)
        pt.pos = V3_NEW(basePos.X, pt.groundY, basePos.Z)
        pt.canReach = AP_CanReach(rootPos, pt.pos, char)
        pt.canShoot = false
        pt.enemySees = false
        pt.score = 0
        if pt.part then
            pt.part.Position = pt.pos + V3_NEW(0, 0.7, 0)
            if pt.canReach then
                pt.part.Color = Color3.new(0.5, 0.5, 0.5)
                pt.part.Transparency = 0.4
                pt.part.Size = V3_NEW(0.5, 0.5, 0.5)
            else
                pt.part.Color = Color3.new(0.35, 0.12, 0.12)
                pt.part.Transparency = 0.7
                pt.part.Size = V3_NEW(0.35, 0.35, 0.35)
            end
        end
    end
end

local function AP_FindBest(myRoot, myChar, enemyRoot, enemyChar)
    local enemyHead = enemyChar:FindFirstChild("Head")
    if not enemyHead then return nil end
    local best = nil
    local bestScore = -999999
    local myPos = myRoot.Position
    local enemyPos = enemyRoot.Position
    for i = 1, #apPoints do
        local pt = apPoints[i]
        if not pt then continue end
        if not pt.canReach then
            if pt.part then pt.part.Color = Color3.fromRGB(180, 40, 40) pt.part.Transparency = 0.7 pt.part.Size = V3_NEW(0.4, 0.4, 0.4) end
            continue
        end
        pt.canShoot = AP_CanShootFrom(pt.pos, enemyHead, myChar, enemyChar)
        pt.enemySees = AP_EnemySeesPoint(enemyRoot, pt.pos, myChar, enemyChar)
        if pt.part then
            if pt.enemySees and pt.canShoot then pt.part.Color = Color3.fromRGB(0, 255, 255) pt.part.Transparency = 0.2 pt.part.Size = V3_NEW(0.7, 0.7, 0.7)
            elseif pt.canShoot then pt.part.Color = Color3.fromRGB(50, 255, 50) pt.part.Transparency = 0.4 pt.part.Size = V3_NEW(0.55, 0.55, 0.55)
            elseif pt.enemySees then pt.part.Color = Color3.fromRGB(255, 150, 0) pt.part.Transparency = 0.45 pt.part.Size = V3_NEW(0.5, 0.5, 0.5)
            else pt.part.Color = Color3.fromRGB(120, 120, 120) pt.part.Transparency = 0.6 pt.part.Size = V3_NEW(0.45, 0.45, 0.45) end
        end
        if pt.enemySees and pt.canShoot then
            local distMe = (pt.pos - myPos).Magnitude
            local distEnemy = (pt.pos - enemyPos).Magnitude
            pt.score = 1000 - distMe * 8 - distEnemy * 3 + (AP_RINGS - pt.ring + 1) * 50
            local toEnemy = (enemyPos - myPos).Unit
            local toPoint = (pt.pos - myPos).Unit
            if toEnemy:Dot(toPoint) < 0.5 then pt.score = pt.score + 80 end
            if pt.score > bestScore then bestScore = pt.score best = pt end
        end
    end
    if best and best.part then best.part.Color = Color3.fromRGB(255, 255, 0) best.part.Size = V3_NEW(0.9, 0.9, 0.9) best.part.Transparency = 0.1 end
    return best
end

local function AP_HasPossiblePath(myRoot, myChar, enemyRoot, enemyChar)
    local enemyHead = enemyChar:FindFirstChild("Head")
    if not enemyHead then return false end
    APRayP.FilterDescendantsInstances = {myChar, enemyChar}
    local myEye = myRoot.Position + V3_NEW(0, 1.6, 0)
    local direct = WS:Raycast(myEye, enemyHead.Position - myEye, APRayP)
    if not direct or not direct.Instance.CanCollide then return true end
    for i = 1, #apPoints do
        local pt = apPoints[i]
        if pt and pt.canReach then
            local eyePos = pt.pos + V3_NEW(0, 1.6, 0)
            local result = WS:Raycast(eyePos, enemyHead.Position - eyePos, APRayP)
            if not result or not result.Instance.CanCollide then return true end
        end
    end
    return false
end

local function AP_FindEnemy(myChar, myRoot)
    local best, bestDist, bestChar = nil, S.apRange, nil
    local myTeam, myColor = LP.Team, LP.TeamColor
    for _, p in ipairs(Plrs:GetPlayers()) do
        if p == LP then continue end
        if S.apTeamCheck and myTeam and (p.Team == myTeam or p.TeamColor == myColor) then continue end
        local c = p.Character
        if not c then continue end
        local r = c:FindFirstChild("HumanoidRootPart")
        local h = c:FindFirstChildOfClass("Humanoid")
        local head = c:FindFirstChild("Head")
        if not r or not h or h.Health <= 0 or not head then continue end
        local dist = (r.Position - myRoot.Position).Magnitude
        if dist >= bestDist then continue end
        if AP_HasPossiblePath(myRoot, myChar, r, c) then bestDist, best, bestChar = dist, r, c end
    end
    return best, bestChar, bestDist
end

local function AP_MoveTo(root, target, char, baseY)
    local start = root.Position
    local look = root.CFrame.LookVector
    local dist = (target - start).Magnitude
    local totalTime, steps, instant = AP_GetMoveParams(dist)
    if instant then
        local finalY = AP_ValidateHeight(baseY, target.Y)
        root.CFrame = CFrame.new(V3_NEW(target.X, finalY, target.Z), V3_NEW(target.X, finalY, target.Z) + look)
        return true
    end
    local stepTime = totalTime / steps
    APRayP.FilterDescendantsInstances = {char}
    for i = 1, steps do
        if not R.apActive or not root.Parent then return false end
        local t = i / steps
        local smooth = AP_SmoothStep(t)
        local pos = start:Lerp(target, smooth)
        local gr = WS:Raycast(pos + V3_NEW(0, 4, 0), V3_NEW(0, -8, 0), APRayP)
        if gr then
            local gy = gr.Position.Y + AP_GROUND_OFFSET
            gy = AP_ValidateHeight(baseY, gy)
            pos = V3_NEW(pos.X, gy, pos.Z)
        else
            pos = V3_NEW(pos.X, AP_ValidateHeight(baseY, pos.Y), pos.Z)
        end
        root.CFrame = CFrame.new(pos, pos + look)
        task.wait(stepTime)
    end
    return true
end

local function AP_DoPeek(root, point, char, enemyRoot, enemyChar)
    if R.apTeleporting then return end
    if S.apCooldown > 0 and TICK() - R.apLastTP < S.apCooldown then return end
    local enemyHead = enemyChar:FindFirstChild("Head")
    if not AP_CanShootFrom(point.pos, enemyHead, char, enemyChar) then return end
    local enemyHum = enemyChar:FindFirstChildOfClass("Humanoid")
    if not enemyHum or enemyHum.Health <= 0 then return end
    R.apTeleporting = true
    R.apLastTP = TICK()
    local baseY = root.Position.Y
    local origin = root.CFrame
    local targetY = AP_ValidateHeight(baseY, point.groundY + AP_GROUND_OFFSET)
    local targetPos = V3_NEW(point.pos.X, targetY, point.pos.Z)
    if point.part then point.part.Color = Color3.fromRGB(255, 0, 255) point.part.Size = V3_NEW(1.2, 1.2, 1.2) point.part.Transparency = 0 end
    if not AP_MoveTo(root, targetPos, char, baseY) then R.apTeleporting = false return end
    if root.Parent and enemyRoot.Parent then
        root.CFrame = CFrame.new(root.Position, V3_NEW(enemyRoot.Position.X, root.Position.Y, enemyRoot.Position.Z))
        task.wait(AP_SHOOT_DELAY)
    end
    if root.Parent then AP_MoveTo(root, origin.Position, char, baseY) root.CFrame = origin end
    if point.part then point.part.Size = V3_NEW(0.5, 0.5, 0.5) end
    R.apTeleporting = false
end

local function AP_MainLoop()
    local loopCounter = 0
    while R.apActive and R.running do
        -- Invalid code Invalid code Invalid code 0.04 Invalid code 0.05
        task.wait(0.05)
        loopCounter = loopCounter + 1
        
        if R.apTeleporting then continue end
        local char = LP.Character
        local root = char and char:FindFirstChild("HumanoidRootPart")
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        if not root or not hum or hum.Health <= 0 then continue end
        local baseY = root.Position.Y
        
        -- Invalid code Invalid code Invalid code
        if loopCounter % 2 == 0 then
            AP_UpdatePoints(root.Position, char, baseY)
        end
        
        local now = TICK()
        -- ESP Invalid code Invalid code
        if now - apLastUpdate > 0.3 then apLastUpdate = now AP_UpdateESP() end
        local enemyRoot, enemyChar, enemyDist = AP_FindEnemy(char, root)
        if not enemyRoot or not enemyChar then
            for i = 1, #apPoints do
                local pt = apPoints[i]
                if pt and pt.part then
                    if pt.canReach then pt.part.Color = Color3.fromRGB(255, 255, 255) pt.part.Transparency = 0.5 pt.part.Size = V3_NEW(0.5, 0.5, 0.5)
                    else pt.part.Color = Color3.fromRGB(100, 40, 40) pt.part.Transparency = 0.75 pt.part.Size = V3_NEW(0.35, 0.35, 0.35) end
                end
            end
            continue
        end
        if AP_EnemyVisible(root, char, enemyChar) then
            for i = 1, #apPoints do
                local pt = apPoints[i]
                if pt and pt.part then pt.part.Color = Color3.fromRGB(100, 200, 100) pt.part.Transparency = 0.6 pt.part.Size = V3_NEW(0.45, 0.45, 0.45) end
            end
            continue
        end
        local best = AP_FindBest(root, char, enemyRoot, enemyChar)
        if best and best.canShoot then AP_DoPeek(root, best, char, enemyRoot, enemyChar) end
    end
    AP_ClearESP()
end

local function AP_Enable()
    if R.apActive then return end
    local char = LP.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    R.apActive = true
    R.apTeleporting = false
    R.apLastTP = 0
    R.apPointCount = 0
    apLastUpdate = 0
    AP_CreatePoints()
    R.hotkeys["AI Peek v4"] = {active = true, key = S.apKey and S.apKey.Name or "None"}
    UpdateHotkeyList()
    task.spawn(AP_MainLoop)
end

local function AP_Disable()
    R.apActive = false
    R.apTeleporting = false
    AP_RemovePoints()
    AP_ClearESP()
    R.hotkeys["AI Peek v4"] = nil
    UpdateHotkeyList()
end

-- ========== GHOST PEEK V9 - FULL POWER (FROM ANTIAIM.LUA) ==========

local GP_COOLDOWN = 0.035  -- 35Invalid code Invalid code Invalid code (Invalid code Invalid code Invalid code)

local gpPosCache = {
    pos = nil,
    enemyPos = nil,
    time = 0,
    enemyId = nil
}

local gpVisualSphere = nil
local gpVisualRing = nil
local gpAuraOrbs = {}  -- Invalid code Invalid code Invalid code Invalid code
local gpLastGoodAngle = 0

local function GP_GetParams()
    local q = (S.gpQuality or 50) / 100
    return {
        maxPoints = FLOOR(40 + q * 100),
        loopDelay = 0.02 - q * 0.015,
        cacheTime = 0.08 - q * 0.05,
        minScore = 80 - q * 60
    }
end

local function GP_CreateVisuals()
    if not gpVisualRing then
        gpVisualRing = Instance.new("Part")
        gpVisualRing.Name = "GP_Ring"
        gpVisualRing.Shape = Enum.PartType.Cylinder
        gpVisualRing.Size = V3_NEW(0.2, 4, 4)
        gpVisualRing.Anchored = true
        gpVisualRing.CanCollide = false
        gpVisualRing.CastShadow = false
        gpVisualRing.Material = Enum.Material.Neon
        gpVisualRing.Color = Color3.fromRGB(0, 255, 100)
        gpVisualRing.Transparency = 1
        gpVisualRing.Orientation = V3_NEW(0, 0, 90)
        gpVisualRing.Parent = WS
    end
    
    if not gpVisualSphere then
        gpVisualSphere = Instance.new("Part")
        gpVisualSphere.Name = "GP_Sphere"
        gpVisualSphere.Shape = Enum.PartType.Ball
        gpVisualSphere.Size = V3_NEW(2, 2, 2)
        gpVisualSphere.Anchored = true
        gpVisualSphere.CanCollide = false
        gpVisualSphere.CastShadow = false
        gpVisualSphere.Material = Enum.Material.ForceField
        gpVisualSphere.Color = Color3.fromRGB(0, 255, 100)
        gpVisualSphere.Transparency = 1
        gpVisualSphere.Parent = WS
    end
    
    -- Invalid code 3 Invalid code Invalid code Invalid code Invalid code
    if #gpAuraOrbs == 0 then
        for i = 1, 3 do
            local orb = Instance.new("Part")
            orb.Name = "GP_AuraOrb_" .. i
            orb.Shape = Enum.PartType.Ball
            orb.Size = V3_NEW(0.8, 0.8, 0.8)
            orb.Anchored = true
            orb.CanCollide = false
            orb.CastShadow = false
            orb.Material = Enum.Material.ForceField
            orb.Color = Color3.fromRGB(0, 255, 100)
            orb.Transparency = 1
            orb.Parent = WS
            table.insert(gpAuraOrbs, orb)
        end
    end
end

local function GP_UpdateVisuals(peekPos, isShooting, myPos)
    if not gpVisualSphere then GP_CreateVisuals() end
    
    -- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
    if myPos and #gpAuraOrbs > 0 then
        local baseSpeed = 90  -- Invalid code Invalid code Invalid code
        local radius = 2.5  -- Invalid code Invalid code
        local verticalOffset = 1.0  -- Invalid code Invalid code Invalid code
        
        -- Invalid code Invalid code Invalid code Invalid code
        local auraColor
        if peekPos then
            auraColor = isShooting and Color3.fromRGB(255, 255, 0) or Color3.fromRGB(0, 255, 100)
        else
            auraColor = Color3.fromRGB(255, 165, 0)
        end
        
        for i, orb in ipairs(gpAuraOrbs) do
            -- Invalid code Invalid code Invalid code Invalid code 120 Invalid code
            local angleOffset = (i - 1) * 120
            local rotAngle = (TICK() * baseSpeed + angleOffset) % 360
            
            -- Invalid code Invalid code Invalid code Invalid code Invalid code
            local vertWave = SIN(TICK() * 3 + i) * 0.3
            
            local offsetX = COS(RAD(rotAngle)) * radius
            local offsetZ = SIN(RAD(rotAngle)) * radius
            local offsetY = verticalOffset + vertWave
            
            orb.Position = V3_NEW(myPos.X + offsetX, myPos.Y + offsetY, myPos.Z + offsetZ)
            orb.Transparency = 0.35  -- Invalid code Invalid code
            orb.Color = auraColor
            
            -- Invalid code Invalid code (Invalid code)
            local pulse = 1.0 + SIN(TICK() * 5 + i * 2) * 0.25
            orb.Size = V3_NEW(pulse, pulse, pulse)
        end
    elseif #gpAuraOrbs > 0 then
        for _, orb in ipairs(gpAuraOrbs) do
            orb.Transparency = 1
        end
    end
    
    if peekPos then
        gpVisualSphere.Position = peekPos + V3_NEW(0, 1.5, 0)
        gpVisualSphere.Transparency = isShooting and 0.2 or 0.5
        gpVisualSphere.Color = isShooting and Color3.fromRGB(255, 255, 0) or Color3.fromRGB(0, 255, 100)
        
        local pulse = 1 + SIN(TICK() * 8) * 0.1
        gpVisualSphere.Size = V3_NEW(2 * pulse, 2 * pulse, 2 * pulse)
        
        gpVisualRing.Position = V3_NEW(peekPos.X, peekPos.Y + 0.1, peekPos.Z)
        gpVisualRing.Transparency = 0.4
        gpVisualRing.Orientation = V3_NEW(0, (TICK() * 60) % 360, 90)
        gpVisualRing.Color = Color3.fromRGB(255, 100, 0)
    else
        gpVisualSphere.Transparency = 1
        gpVisualRing.Transparency = 1
    end
end

local function GP_RemoveVisuals()
    if gpVisualSphere then pcall(function() gpVisualSphere:Destroy() end) gpVisualSphere = nil end
    if gpVisualRing then pcall(function() gpVisualRing:Destroy() end) gpVisualRing = nil end
    for _, orb in ipairs(gpAuraOrbs) do
        pcall(function() orb:Destroy() end)
    end
    gpAuraOrbs = {}
end

local function GP_FindPeekPosition(myRoot, myChar, enemyHead, enemyChar)
    if not myRoot or not enemyHead then return nil end
    
    local params = GP_GetParams()
    local now = TICK()
    local enemyId = enemyChar and tostring(enemyChar) or nil
    
    local myPos = myRoot.Position
    local enemyPos = enemyHead.Position
    local distToEnemy = (enemyPos - myPos).Magnitude
    
    if gpPosCache.pos and gpPosCache.enemyId == enemyId then
        if now - gpPosCache.time < params.cacheTime then
            local moved = gpPosCache.enemyPos and (enemyHead.Position - gpPosCache.enemyPos).Magnitude or HUGE
            if moved < 2 then return gpPosCache.pos end
        end
    end
    
    local dirToEnemy = enemyPos - myPos
    local flatDir = V3_NEW(dirToEnemy.X, 0, dirToEnemy.Z)
    if flatDir.Magnitude < 0.1 then return nil end
    flatDir = flatDir.Unit
    
    local peekDist = S.gpPeekDist
    local maxHeight = S.gpHeight
    
    local best, bestScore = nil, -HUGE
    local pointsTested = 0
    
    APRayP.FilterDescendantsInstances = {myChar, enemyChar}
    RayP.FilterDescendantsInstances = {myChar}
    
    if gpLastGoodAngle ~= 0 then
        local quickAngles = {gpLastGoodAngle, gpLastGoodAngle + 15, gpLastGoodAngle - 15, gpLastGoodAngle + 30, gpLastGoodAngle - 30, gpLastGoodAngle + 60, gpLastGoodAngle - 60}
        for _, angle in ipairs(quickAngles) do
            local rad = RAD(angle)
            local rotatedDir = V3_NEW(
                flatDir.X * COS(rad) - flatDir.Z * SIN(rad),
                0,
                flatDir.X * SIN(rad) + flatDir.Z * COS(rad)
            )
            
            for distMult = 0.1, 0.9, 0.1 do
                local dist = peekDist * distMult
                
                local testPos = myPos + rotatedDir * dist
                
                local groundRay = WS:Raycast(testPos + V3_NEW(0, 5, 0), V3_NEW(0, -10, 0), APRayP)
                if not groundRay then continue end
                
                local finalY = CLAMP(groundRay.Position.Y + 2.8, myPos.Y - maxHeight, myPos.Y + maxHeight)
                testPos = V3_NEW(testPos.X, finalY, testPos.Z)
                
                local pathRay = WS:Raycast(myPos + V3_NEW(0, 1.5, 0), (testPos + V3_NEW(0, 1.5, 0)) - (myPos + V3_NEW(0, 1.5, 0)), APRayP)
                if pathRay and pathRay.Instance and pathRay.Instance.CanCollide then continue end
                
                local eyePos = testPos + V3_NEW(0, 1.5, 0)
                local res = WS:Raycast(eyePos, enemyPos - eyePos, RayP)
                
                if not res or res.Instance:IsDescendantOf(enemyChar) then
                    gpPosCache.pos = testPos
                    gpPosCache.enemyPos = enemyHead.Position
                    gpPosCache.time = now
                    gpPosCache.enemyId = enemyId
                    return testPos
                end
            end
        end
    end
    
    local numDistSteps = CLAMP(FLOOR(peekDist / 2), 5, 20)
    local numAngleSteps = CLAMP(FLOOR(peekDist), 12, 36)
    
    for distIdx = 1, numDistSteps do
        if pointsTested >= params.maxPoints then break end
        if best and bestScore > 320 then break end
        
        local distRatio = distIdx / numDistSteps
        local dist = 1.5 + (peekDist - 1.5) * distRatio
        
        if dist > distToEnemy * 0.85 then continue end
        
        for angleIdx = 1, numAngleSteps do
            if pointsTested >= params.maxPoints then break end
            pointsTested = pointsTested + 1
            
            -- Invalid code Invalid code -110 Invalid code +110 Invalid code (Invalid code Invalid code Invalid code)
            local angle = -110 + (220 * (angleIdx - 1) / (numAngleSteps - 1))
            local rad = RAD(angle)
            
            local rotatedDir = V3_NEW(
                flatDir.X * COS(rad) - flatDir.Z * SIN(rad),
                0,
                flatDir.X * SIN(rad) + flatDir.Z * COS(rad)
            )
            
            local testPos = myPos + rotatedDir * dist
            
            local groundRay = WS:Raycast(testPos + V3_NEW(0, 5, 0), V3_NEW(0, -10, 0), APRayP)
            if not groundRay then continue end
            
            local finalY = CLAMP(groundRay.Position.Y + 2.8, myPos.Y - maxHeight, myPos.Y + maxHeight)
            testPos = V3_NEW(testPos.X, finalY, testPos.Z)
            
            local pathClear = true
            for _, checkY in ipairs({1.0, 2.2}) do
                local pathRay = WS:Raycast(
                    myPos + V3_NEW(0, checkY, 0), 
                    (testPos + V3_NEW(0, checkY, 0)) - (myPos + V3_NEW(0, checkY, 0)), 
                    APRayP
                )
                if pathRay and pathRay.Instance.CanCollide then 
                    pathClear = false 
                    break 
                end
            end
            
            if not pathClear then continue end
            
            local eyePos = testPos + V3_NEW(0, 1.5, 0)
            local res = WS:Raycast(eyePos, enemyPos - eyePos, RayP)
            
            if res and not res.Instance:IsDescendantOf(enemyChar) then
                continue
            end
            
            local distFromMe = (testPos - myPos).Magnitude
            local distFromEnemy = (testPos - enemyPos).Magnitude
            
            local score = 350 - distFromMe * 1.2
            
            if distFromEnemy >= 8 and distFromEnemy <= 30 then
                score = score + 60
            elseif distFromEnemy > 30 and distFromEnemy <= 50 then
                score = score + 40
            elseif distFromEnemy > 50 then
                score = score + 20
            end
            
            if ABS(angle) >= 20 and ABS(angle) <= 90 then 
                score = score + 55
            end
            
            local heightAdvantage = testPos.Y - enemyPos.Y
            if heightAdvantage > 0 and heightAdvantage <= maxHeight then
                score = score + MIN(35, heightAdvantage * 10)
            end
            
            if distFromMe <= peekDist * 0.4 then
                score = score + 35
            end
            
            if score > bestScore then
                bestScore = score
                best = testPos
                gpLastGoodAngle = angle
            end
        end
    end
    
    gpPosCache.pos = best
    gpPosCache.enemyPos = enemyHead.Position
    gpPosCache.time = now
    gpPosCache.enemyId = enemyId
    
    return best
end

local function GP_FindTarget(myChar, myRoot)
    local best, bestDist, bestChar, bestHead, bestData = nil, S.gpRange, nil, nil, nil
    local myTeam, myColor = LP.Team, LP.TeamColor
    
    for _, p in ipairs(Plrs:GetPlayers()) do
        if p ~= LP then
            -- Invalid code Invalid code
            if S.gpTeamCheck and myTeam and (p.Team == myTeam or p.TeamColor == myColor) then continue end
            
            local c = p.Character
            if not c then continue end
            
            local r = c:FindFirstChild("HumanoidRootPart")
            local h = c:FindFirstChildOfClass("Humanoid")
            local head = c:FindFirstChild("Head")
            
            if r and h and head and h.Health > 0 then
                local dist = (myRoot.Position - r.Position).Magnitude
                if dist < bestDist then
                    bestDist = dist
                    best = r
                    bestChar = c
                    bestHead = head
                    bestData = {p = p, c = c, r = r, head = head, dist = dist}
                end
            end
        end
    end
    
    return best, bestChar, bestHead, bestData
end

local function GP_DoGhostShot(root, peekPos, enemyHead, myChar, enemyChar, enemyData)
    if TICK() - R.gpLastShot < GP_COOLDOWN then return false end
    
    local fs = GetFireShot()
    if not fs then return false end
    
    local shootOrigin = peekPos + V3_NEW(0, 1.5, 0)
    
    -- PREDICTION: Invalid code Invalid code Invalid code
    local targetPos = enemyHead.Position
    if enemyData and enemyData.r then
        local vel = enemyData.r.AssemblyLinearVelocity
        if vel.Magnitude > 0.5 then
            local ping = LP:GetNetworkPing()
            local predTime = ping * 1.1
            targetPos = enemyHead.Position + V3_NEW(vel.X * predTime, 0, vel.Z * predTime)
        end
    end
    
    -- Invalid code Invalid code Invalid code prediction
    RayP.FilterDescendantsInstances = {myChar}
    local res = WS:Raycast(shootOrigin, targetPos - shootOrigin, RayP)
    
    if res and not res.Instance:IsDescendantOf(enemyChar) then
        -- Invalid code Invalid code prediction
        res = WS:Raycast(shootOrigin, enemyHead.Position - shootOrigin, RayP)
        if res and not res.Instance:IsDescendantOf(enemyChar) then
            return false
        end
        targetPos = enemyHead.Position
    end
    
    local originalCF = root.CFrame
    local originalVel = root.AssemblyLinearVelocity
    local myPos = root.Position
    
    R.gpInPeek = true
    GP_UpdateVisuals(peekPos, true, myPos)
    
    -- Invalid code Invalid code Invalid code Invalid code
    local lookAt = V3_NEW(targetPos.X, peekPos.Y, targetPos.Z)
    root.CFrame = CFrame.new(peekPos, lookAt)
    root.AssemblyLinearVelocity = V3_ZERO
    
    -- Invalid code Invalid code prediction
    local direction = (targetPos - shootOrigin).Unit
    pcall(function() fs:FireServer(shootOrigin, direction, enemyHead) end)
    
    -- Invalid code Invalid code
    root.CFrame = originalCF
    root.AssemblyLinearVelocity = originalVel
    
    R.gpInPeek = false
    R.gpLastShot = TICK()
    
    -- Invalid code Invalid code
    local playerName = enemyData and enemyData.p and enemyData.p.Name or "Unknown"
    TrackShot(playerName, "Head")
    
    task.delay(0.02, function()
        if R.gpActive then
            GP_UpdateVisuals(peekPos, false, myPos)
        end
    end)
    
    return true
end

local function GP_Enable()
    if R.gpActive then return end
    
    CacheChar()
    
    local char = LP.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    
    R.gpActive = true
    R.gpLastShot = 0
    R.gpInPeek = false
    
    gpPosCache.pos = nil
    gpPosCache.time = 0
    gpLastGoodAngle = 0
    
    GP_CreateVisuals()
    
    R.hotkeys["Ghost Peek"] = {active = true, key = S.gpKey.Name}
    UpdateHotkeyList()
    
    task.spawn(function()
        local params = GP_GetParams()
        local frameSkip = 0
        local lastPeekPos = nil
        local lastEnemyId = nil  -- Invalid code Invalid code Invalid code
        
        while R.gpActive and R.running do
            params = GP_GetParams()
            task.wait(params.loopDelay)
            
            if R.gpInPeek then continue end
            
            CacheChar()
            
            local char = LP.Character
            local root = char and char:FindFirstChild("HumanoidRootPart")
            local hum = char and char:FindFirstChildOfClass("Humanoid")
            
            if not root or not hum or hum.Health <= 0 then
                GP_UpdateVisuals(nil, false, nil)
                lastPeekPos = nil
                lastEnemyId = nil
                continue
            end
            
            local myPos = root.Position
            local enemyRoot, enemyChar, enemyHead, enemyData = GP_FindTarget(char, root)
            
            if not enemyRoot or not enemyHead then
                GP_UpdateVisuals(nil, false, myPos)
                lastPeekPos = nil
                lastEnemyId = nil
                continue
            end
            
            -- Invalid code Invalid code Invalid code - Invalid code Invalid code Invalid code Invalid code Invalid code
            local currentEnemyId = tostring(enemyChar)
            if currentEnemyId ~= lastEnemyId then
                lastPeekPos = nil
                frameSkip = 0
                gpPosCache.pos = nil
                gpPosCache.time = 0
                gpPosCache.enemyId = nil
                gpLastGoodAngle = 0
                lastEnemyId = currentEnemyId
            end
            
            -- Invalid code: Invalid code Invalid code Invalid code? (Invalid code Invalid code Invalid code Invalid code antiaim.lua)
            RayP.FilterDescendantsInstances = {char}
            local eyePos = root.Position + V3_NEW(0, 1.5, 0)
            local directRay = WS:Raycast(eyePos, enemyHead.Position - eyePos, RayP)
            
            -- Invalid code Invalid code Invalid code Invalid code - Invalid code Invalid code ghost peek (Invalid code Invalid code Invalid code)
            if not directRay or directRay.Instance:IsDescendantOf(enemyChar) then
                GP_UpdateVisuals(nil, false, myPos)
                lastPeekPos = nil
                continue
            end
            
            -- Invalid code Invalid code Invalid code - Invalid code Invalid code Invalid code Invalid code
            frameSkip = frameSkip + 1
            local peekPos = lastPeekPos
            
            if not lastPeekPos or frameSkip >= 2 then
                frameSkip = 0
                peekPos = GP_FindPeekPosition(root, char, enemyHead, enemyChar)
                lastPeekPos = peekPos
            end
            
            if peekPos then
                GP_UpdateVisuals(peekPos, false, myPos)
                if S.gpAutoshoot then
                    GP_DoGhostShot(root, peekPos, enemyHead, char, enemyChar, enemyData)
                end
            else
                GP_UpdateVisuals(nil, false, myPos)
            end
        end
        
        GP_RemoveVisuals()
    end)
end

local function GP_Disable()
    R.gpActive = false
    R.gpInPeek = false
    gpPosCache.pos = nil
    gpLastGoodAngle = 0
    GP_RemoveVisuals()
    R.hotkeys["Ghost Peek"] = nil
    UpdateHotkeyList()
end


-- ========== EXPLOIT POSITION SYSTEM (FIXED) ==========
local function EP_SpawnDecoy()
    if R.epActive then return end
    if not R.myChar or not R.myHRP or not R.myHum then return end
    
    R.epActive = true
    R.epOriginalPos = R.myHRP.CFrame
    
    if R.epDecoy then pcall(function() R.epDecoy:Destroy() end) end
    
    local cam = WS.CurrentCamera
    local camLook = cam and cam.CFrame.LookVector or R.myHRP.CFrame.LookVector
    camLook = V3_NEW(camLook.X, 0, camLook.Z).Unit
    
    local newPos = R.epOriginalPos.Position + camLook * S.epDist
    R.myHRP.CFrame = CFrame.new(newPos) * CFrame.Angles(0, math.atan2(-camLook.X, -camLook.Z), 0)
    
    local newModel = Instance.new("Model")
    newModel.Name = "ShadowDecoy"
    newModel.Parent = WS
    R.epDecoy = newModel
    
    for _, obj in pairs(R.myChar:GetChildren()) do
        if obj:IsA("BasePart") and obj.Name ~= "HumanoidRootPart" then
            local partClone = obj:Clone()
            partClone.Name = obj.Name
            partClone.Parent = R.epDecoy
            partClone.Transparency = 0.3
            partClone.Color = Color3.fromRGB(20, 20, 20)
            partClone.CanCollide = false
            partClone.Material = Enum.Material.ForceField
            partClone.Anchored = false
        end
    end
    
    local hrpClone = R.myHRP:Clone()
    hrpClone.Name = "HumanoidRootPart"
    hrpClone.Anchored = true
    hrpClone.CFrame = R.epOriginalPos
    hrpClone.Transparency = 0.3
    hrpClone.Color = Color3.fromRGB(20, 20, 20)
    hrpClone.CanCollide = false
    hrpClone.Parent = R.epDecoy
    
    local hum = Instance.new("Humanoid")
    hum.MaxHealth = 0
    hum.Health = 0
    hum.DisplayDistanceType = Enum.HumanoidDisplayDistanceType.None
    hum.Parent = R.epDecoy
    
    for _, part in pairs(R.epDecoy:GetChildren()) do
        if part:IsA("BasePart") and part ~= hrpClone then
            local weld = Instance.new("WeldConstraint")
            weld.Part0 = hrpClone
            weld.Part1 = part
            weld.Parent = hrpClone
        end
    end
    
    local torso = R.myChar:FindFirstChild("Torso") or R.myChar:FindFirstChild("UpperTorso")
    if torso then
        local neck = torso:FindFirstChild("Neck")
        if neck and neck:IsA("Motor6D") then
            R.epOriginalNeckC0 = neck.C0
            neck.C0 = neck.C0 * CFrame.new(0, -2, 0)
        end
    end
    
    R.epHealthConn = R.myHum.HealthChanged:Connect(function(health)
        if R.epActive and health < R.myHum.MaxHealth then
            pcall(function() R.myHum.Health = R.myHum.MaxHealth end)
        end
    end)
    
    R.hotkeys["Exploit Pos"] = {active = true, key = S.epKey.Name}
    UpdateHotkeyList()
end

local function EP_DestroyDecoy()
    if not R.epActive then return end
    
    if R.epDecoy then pcall(function() R.epDecoy:Destroy() end) R.epDecoy = nil end
    if R.epOriginalPos and R.myHRP then R.myHRP.CFrame = R.epOriginalPos end
    
    local torso = R.myChar and (R.myChar:FindFirstChild("Torso") or R.myChar:FindFirstChild("UpperTorso"))
    if torso then
        local neck = torso:FindFirstChild("Neck")
        if neck and neck:IsA("Motor6D") and R.epOriginalNeckC0 then neck.C0 = R.epOriginalNeckC0 end
    end
    R.epOriginalNeckC0 = nil
    
    if R.epHealthConn then R.epHealthConn:Disconnect() R.epHealthConn = nil end
    
    R.epOriginalPos = nil
    R.epActive = false
    R.hotkeys["Exploit Pos"] = nil
    UpdateHotkeyList()
end

local function EP_Free()
    EP_DestroyDecoy()
    if R.myHum then pcall(function() R.myHum.WalkSpeed = 16 end) end
    if R.myHRP then pcall(function() R.myHRP.Anchored = false end) end
end

-- ========== BARREL EXTEND ==========
local function BE_GetHole()
    if not LP.Character then return nil end
    local playerFolder = WS:FindFirstChild(LP.Name)
    if playerFolder then
        local ssg = playerFolder:FindFirstChild("SSG-08")
        if ssg then
            local hole = ssg:FindFirstChild("Hole")
            if hole and hole:IsA("BasePart") then return hole end
        end
    end
    return nil
end

local function BE_Enable()
    if R.beActive then return end
    if not R.myChar or not R.myHRP then return end
    local hole = BE_GetHole()
    if not hole then return end
    if R.beDecoy then pcall(function() R.beDecoy:Destroy() end) R.beDecoy = nil end
    
    local decoy = Instance.new("Model")
    decoy.Name = "BarrelDecoy"
    decoy.Parent = WS
    R.beDecoy = decoy
    
    local hrpClone = Instance.new("Part")
    hrpClone.Name = "HumanoidRootPart"
    hrpClone.Size = V3_NEW(2, 2, 1)
    hrpClone.Transparency = 0.3
    hrpClone.Color = Color3.fromRGB(20, 20, 20)
    hrpClone.CanCollide = false
    hrpClone.Anchored = true
    hrpClone.CFrame = CFrame.new(hole.Position)
    hrpClone.Parent = decoy
    
    local hum = Instance.new("Humanoid")
    hum.MaxHealth = 0
    hum.Health = 0
    hum.DisplayDistanceType = Enum.HumanoidDisplayDistanceType.None
    hum.Parent = decoy
    
    R.beActive = true
    R.hotkeys["Barrel Ext"] = {active = true, key = S.beKey.Name .. " [" .. S.beMode .. "]"}
    UpdateHotkeyList()
    
    if R.beUpdateConn then R.beUpdateConn:Disconnect() end
    R.beUpdateConn = RS.Heartbeat:Connect(function()
        if R.beActive and R.beDecoy then
            local h = BE_GetHole()
            if h then
                local hrp = R.beDecoy:FindFirstChild("HumanoidRootPart")
                if hrp then hrp.CFrame = CFrame.new(h.Position) end
            end
        end
    end)
end

local function BE_Disable()
    if not R.beActive then return end
    if R.beUpdateConn then R.beUpdateConn:Disconnect() R.beUpdateConn = nil end
    if R.beDecoy then pcall(function() R.beDecoy:Destroy() end) R.beDecoy = nil end
    R.beActive = false
    R.hotkeys["Barrel Ext"] = nil
    UpdateHotkeyList()
end

local function BE_GetShootOrigin(defaultOrigin)
    if not R.beActive then return defaultOrigin end
    local hole = BE_GetHole()
    if hole then return hole.Position end
    return defaultOrigin
end

local PLAYER_CACHE_INTERVAL = 0.2  -- Invalid code: Invalid code 0.15, Invalid code 0.2
local function UpdatePlayerData()
    local now = TICK()
    if now - R.playerDataTime < PLAYER_CACHE_INTERVAL then return end
    R.playerDataTime = now
    
    if not R.myHRP then return end
    local myPos = R.myHRP.Position
    local myTeam, myColor = LP.Team, LP.TeamColor
    local count = 0
    
    -- Invalid code Invalid code Invalid code Invalid code
    for i = 1, 16 do
        R.playerData[i] = nil
    end
    
    for _, p in ipairs(Plrs:GetPlayers()) do
        if p ~= LP then
            local c = p.Character
            if c then
                local h = c:FindFirstChild("Humanoid")
                local r = c:FindFirstChild("HumanoidRootPart")
                if h and h.Health > 0 and r then
                    local dist = (myPos - r.Position).Magnitude
                    if dist < 600 then
                        count = count + 1
                        local isTeam = myTeam and (p.Team == myTeam or p.TeamColor == myColor)
                        R.playerData[count] = {
                            p = p, c = c, h = h, r = r,
                            head = c:FindFirstChild("Head"),
                            torso = c:FindFirstChild("UpperTorso") or c:FindFirstChild("Torso"),
                            dist = dist, team = isTeam,
                            vel = r.AssemblyLinearVelocity
                        }
                    end
                end
            end
        end
    end
    
    -- Invalid code Invalid code Invalid code (Invalid code Invalid code Invalid code Invalid code)
    for i = 2, count do
        local key = R.playerData[i]
        local j = i - 1
        while j >= 1 and R.playerData[j] and R.playerData[j].dist > key.dist do
            R.playerData[j + 1] = R.playerData[j]
            j = j - 1
        end
        R.playerData[j + 1] = key
    end
end

-- ========== INFINITY JUMP SYSTEM ==========
local function IJ_CreatePart()
    if R.ijPart then return end
    if not R.myHRP then return end
    local part = Instance.new("Part")
    part.Name = "InfinityJumpPlatform"
    part.Size = V3_NEW(4, 0.5, 4)
    part.Anchored = true
    part.CanCollide = true
    part.Transparency = 1
    part.CFrame = R.myHRP.CFrame * CFrame.new(0, -3, 0)
    part.Parent = WS
    R.ijPart = part
end

local function IJ_RemovePart()
    if R.ijPart then pcall(function() R.ijPart:Destroy() end) R.ijPart = nil end
end

local function IJ_Update()
    if not S.ijEnabled then IJ_RemovePart() return end
    if not R.myHRP or not R.myHum then return end
    local state = R.myHum:GetState()
    local isInAir = state == Enum.HumanoidStateType.Freefall or state == Enum.HumanoidStateType.Jumping
    if isInAir then
        IJ_CreatePart()
        if R.ijPart then R.ijPart.CFrame = R.myHRP.CFrame * CFrame.new(0, -3, 0) end
        R.ijCanJump = true
    else
        IJ_RemovePart()
        R.ijCanJump = true
    end
end

local function IJ_Jump()
    if not S.ijEnabled then return end
    if not R.myHum or not R.myHRP then return end
    if not R.ijCanJump then return end
    local state = R.myHum:GetState()
    local isInAir = state == Enum.HumanoidStateType.Freefall or state == Enum.HumanoidStateType.Jumping
    if isInAir then
        IJ_CreatePart()
        if R.ijPart then
            R.ijPart.CFrame = R.myHRP.CFrame * CFrame.new(0, -3, 0)
            R.ijPart.CanCollide = true
        end
        task.wait(0.02)
        R.myHum:ChangeState(Enum.HumanoidStateType.Jumping)
        task.delay(0.1, function()
            if R.ijPart then R.ijPart.CanCollide = false end
        end)
    end
end

-- ========== DOUBLE TAP SYSTEM ==========
local function DT_Peek(target)
    if not S.dtEnabled then return end
    if not R.myHRP or not R.myHum then return end
    local now = TICK()
    if now - R.dtLastPeek < 0.3 then return end
    R.dtLastPeek = now
    
    local cam = WS.CurrentCamera
    local camLook = cam and cam.CFrame.LookVector or R.myHRP.CFrame.LookVector
    camLook = V3_NEW(camLook.X, 0, camLook.Z).Unit
    
    local originalPos = R.myHRP.CFrame
    local peekPos = originalPos.Position + camLook * S.dtDist
    
    R.myHRP.CFrame = CFrame.new(peekPos) * CFrame.Angles(0, math.atan2(-camLook.X, -camLook.Z), 0)
    
    task.delay(0.15, function()
        if R.myHRP and R.myHRP.Parent then
            R.myHRP.CFrame = originalPos
        end
    end)
end

-- ========== AIMVIEW SYSTEM ==========
local function AV_CreateLine()
    if R.avLine then pcall(function() R.avLine:Destroy() end) R.avLine = nil end
    local line = Instance.new("Part")
    line.Name = "AimViewLine"
    line.Anchored = true
    line.CanCollide = false
    line.Material = Enum.Material.Neon
    line.Color = S.avColor or Color3.fromRGB(255, 0, 0)
    line.Transparency = 0.3
    line.Parent = WS
    R.avLine = line
end

local function AV_RemoveLine()
    if R.avLine then pcall(function() R.avLine:Destroy() end) R.avLine = nil end
end

local function AV_Update(targetPos, fromPos)
    if not S.avEnabled then AV_RemoveLine() return end
    if not targetPos then AV_RemoveLine() return end
    if not R.avLine or not R.avLine.Parent then AV_CreateLine() end
    if R.avLine then
        R.avLine.Color = S.avColor or Color3.fromRGB(255, 0, 0)
        local startPos = fromPos or (R.myHRP and (R.myHRP.Position + R.myHRP.CFrame.LookVector * 1)) or (R.myHead and R.myHead.Position)
        if not startPos then AV_RemoveLine() return end
        local endPos = targetPos
        local distance = (endPos - startPos).Magnitude
        local midPoint = (startPos + endPos) / 2
        R.avLine.Size = V3_NEW(0.1, 0.1, distance)
        R.avLine.CFrame = CFrame.lookAt(midPoint, endPos)
        R.avTarget = targetPos
    end
end


-- ========== Invalid code Invalid code (ULTRA FAST AIMBOT) ==========
local frame = 0
local lastAimUpdate = 0
local AIM_UPDATE_INTERVAL = 0.016 -- ~60 FPS Invalid code Invalid code

local function MainLoop(dt)
    if not R.running then return end
    frame = frame + 1
    
    -- Invalid code Invalid code Invalid code
    if frame % 15 == 0 then CacheChar() end
    if not R.myChar or not R.myHRP then return end
    
    -- Invalid code Invalid code Invalid code Invalid code Invalid code
    UpdatePlayerData()
    
    local now = TICK()
    local hrp = R.myHRP
    local head = R.myHead
    local cam = R.cam
    
    -- INFINITY JUMP UPDATE (Invalid code 3 Invalid code)
    if S.ijEnabled and frame % 3 == 0 then IJ_Update() end
    
    -- HIT AIR UPDATE (Invalid code 2 Invalid code)
    if R.hitAirActive and frame % 2 == 0 then HitAir_Update() end
    
    -- RAGE BOT (Invalid code Invalid code 2.5 Invalid code Invalid code Invalid code Invalid code Invalid code)
    if S.rbEnabled and S.rbAutoFire and head then
        -- Invalid code Invalid code Invalid code
        local isGrounded = true
        if R.myHum and R.myHRP then
            if R.myHum.FloorMaterial == Enum.Material.Air then isGrounded = false end
            local vel = R.myHRP.AssemblyLinearVelocity or R.myHRP.Velocity
            if ABS(vel.Y) > 2 then isGrounded = false end
        end
        
        -- Invalid code 2.5 Invalid code
        if isGrounded and now - R.rbLast >= 2.5 then
            RayP.FilterDescendantsInstances = {R.myChar}
            local best = nil
            local bestScore = -9999
            local aiTargetPos = nil
            
            local bulletOrigin = hrp.Position + V3_NEW(0, 1.5, 0)
            
            for i = 1, 8 do
                local d = R.playerData[i]
                if d and not d.team and d.dist < S.rbMaxDist then
                    -- NO AIR CHECK Invalid code Invalid code
                    if S.rbNoAir and not R.apActive then
                        local enemyPos = d.r.Position
                        RayP.FilterDescendantsInstances = {d.c}
                        local groundRay = WS:Raycast(enemyPos, V3_NEW(0, -4, 0), RayP)
                        local isInAir = groundRay == nil
                        local enemyVelY = d.vel and d.vel.Y or 0
                        if isInAir or ABS(enemyVelY) > 8 then continue end
                        RayP.FilterDescendantsInstances = {R.myChar}
                    end
                    
                    local targets = {}
                    if S.rbAirShoot then
                        if d.head then table.insert(targets, {part = d.head, priority = 3}) end
                        if d.torso then table.insert(targets, {part = d.torso, priority = 2}) end
                    elseif S.rbSmartAim then
                        if d.head then table.insert(targets, {part = d.head, priority = 3}) end
                        if d.torso then table.insert(targets, {part = d.torso, priority = 2}) end
                        table.insert(targets, {part = d.r, priority = 1})
                    else
                        local tgt = S.rbHitbox == "Head" and d.head or d.torso or d.r
                        if tgt then table.insert(targets, {part = tgt, priority = 1}) end
                    end
                    
                    for _, tgtData in ipairs(targets) do
                        local tgt = tgtData.part
                        if not tgt then continue end
                        
                        local vel = d.vel or d.r.AssemblyLinearVelocity
                        local ping = LP:GetNetworkPing()
                        local playerName = d.p and d.p.Name or "Unknown"
                        local realPos = tgt.Position
                        local targetPos = realPos
                        
                        -- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
                        local params = RaycastParams.new()
                        params.FilterType = Enum.RaycastFilterType.Exclude
                        params.FilterDescendantsInstances = {R.myChar, d.c}
                        
                        local realDirection = realPos - bulletOrigin
                        local res = WS:Raycast(bulletOrigin, realDirection, params)
                        local canSeeReal = (res == nil)
                        
                        if S.rbWallCheck and not canSeeReal then continue end
                        
                        -- Invalid code Invalid code Invalid code Invalid code Invalid code - Invalid code Invalid code
                        if canSeeReal then
                            local aiMode = "TRACK"
                            local aiConfidence = 0.5
                            
                            if S.rbPredMode == "Beta AI" then
                                -- Beta AI Invalid code - Invalid code AI Invalid code
                                local aiPredictedPos
                                aiPredictedPos, aiConfidence, aiMode = AI_GetBestPrediction(playerName, realPos, vel, ping)
                                
                                -- Invalid code Invalid code Invalid code Invalid code
                                local confThreshold = S.aiConfThreshold / 100
                                
                                -- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
                                local predDirection = aiPredictedPos - bulletOrigin
                                local predRes = WS:Raycast(bulletOrigin, predDirection, params)
                                local canSeePred = (predRes == nil)
                                
                                -- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
                                if canSeePred and aiConfidence >= confThreshold then
                                    targetPos = aiPredictedPos
                                else
                                    -- Invalid code Invalid code Invalid code
                                    targetPos = realPos + vel * ping
                                    aiConfidence = 0.5
                                end
                                
                                -- Invalid code Invalid code Invalid code Invalid code
                                if S.aiVisualBox or S.aiVisualTrace then
                                    AI_ShowVisuals(playerName, realPos, targetPos, aiConfidence, aiMode)
                                end
                            else
                                -- Default Invalid code - Invalid code Invalid code Invalid code Invalid code
                                targetPos = realPos + vel * ping
                            end
                            
                            local score = (S.rbMaxDist - d.dist) + tgtData.priority * 100
                            if score > bestScore then
                                bestScore = score
                                best = {d = d, tgt = tgt, predictedPos = targetPos, aiConfidence = aiConfidence, aiMode = aiMode}
                                aiTargetPos = targetPos
                            end
                            break
                        end
                    end
                end
            end
            
            -- AimView
            local aimViewOrigin = bulletOrigin
            if R.beActive then
                local hole = BE_GetHole()
                if hole then aimViewOrigin = hole.Position end
            end
            
            if S.avEnabled and best and best.d and best.d.head then
                AV_Update(best.d.head.Position, aimViewOrigin)
            elseif S.avEnabled then
                AV_RemoveLine()
            end
            
            if best then
                local fs = GetFireShot()
                if fs then
                    local pos = best.predictedPos or best.tgt.Position
                    local shootOrigin = BE_GetShootOrigin(bulletOrigin)
                    local direction = (pos - shootOrigin).Unit
                    
                    local targetName = best.d.p and best.d.p.Name or "Unknown"
                    local hitboxName = best.tgt.Name or "Body"
                    
                    pcall(function()
                        fs:FireServer(shootOrigin, direction, best.tgt)
                    end)
                    R.rbLast = now
                    
                    TrackShot(targetName, hitboxName)
                end
            end
            
            -- AUTO DOUBLE TAP: Invalid code Invalid code Invalid code Invalid code 1 Invalid code Invalid code
            if S.dtAuto and S.dtEnabled and now - R.dtAutoLast > (S.dtAutoDelay / 1000) then
                local visibleCount = 0
                local visibleEnemy = nil
                
                for i = 1, 8 do
                    local d = R.playerData[i]
                    if d and not d.team and d.dist < S.rbMaxDist then
                        -- Invalid code Invalid code (Invalid code Invalid code Invalid code)
                        RayP.FilterDescendantsInstances = {R.myChar}
                        local tgt = d.head or d.torso or d.r
                        if tgt then
                            local res = WS:Raycast(bulletOrigin, tgt.Position - bulletOrigin, RayP)
                            if not res or res.Instance:IsDescendantOf(d.c) then
                                visibleCount = visibleCount + 1
                                visibleEnemy = d
                            end
                        end
                    end
                end
                
                -- Invalid code Invalid code 1 Invalid code Invalid code Invalid code Invalid code "full open" - Invalid code DT
                if visibleCount == 1 and visibleEnemy then
                    R.dtAutoLast = now
                    DT_Peek(nil)
                    DC_Log("Auto DT triggered on " .. (visibleEnemy.p and visibleEnemy.p.Name or "enemy"), "info")
                end
            end
        end
    end
    
    -- BUNNYHOP (Invalid code 3 Invalid code Invalid code 2)
    if S.bhEnabled and frame % 3 == 0 and R.myHum then
        if now - R.bhPosCheckTime >= 1.5 then
            local curPos = hrp.Position
            if R.bhLastPos then
                local dist = (V3_NEW(curPos.X, 0, curPos.Z) - V3_NEW(R.bhLastPos.X, 0, R.bhLastPos.Z)).Magnitude
                R.bhCircling = dist < 15
            end
            R.bhLastPos, R.bhPosCheckTime = curPos, now
        end
        
        if not R.bhCircling then
            if now - R.bhLastReset >= 2.5 and not R.bhResetting then
                R.bhResetting, R.bhLastReset = true, now
                local wasInAir = R.bhInAir
                R.myHum.WalkSpeed = 27
                task.delay(0.2, function()
                    if S.bhEnabled and R.myHum then
                        if wasInAir and R.bhInAir then R.myHum.WalkSpeed = S.bhAirSpeed
                        elseif not R.bhInAir then R.myHum.WalkSpeed = S.bhGroundSpeed end
                    end
                    R.bhResetting = false
                end)
            end
            
            local rayOrigin = hrp.Position
            local rayDirection = V3_NEW(0, -3.5, 0)
            RayP.FilterDescendantsInstances = {R.myChar}
            local rayRes = WS:Raycast(rayOrigin, rayDirection, RayP)
            local onGround = rayRes ~= nil
            
            if not onGround and not R.bhInAir then
                R.bhInAir = true
                if not R.bhResetting then R.myHum.WalkSpeed = S.bhAirSpeed end
            elseif onGround and R.bhInAir then
                R.bhInAir = false
                if not R.bhResetting then R.myHum.WalkSpeed = S.bhGroundSpeed end
            end
            
            if R.bhInAir and not R.bhResetting then
                local vel = hrp.AssemblyLinearVelocity
                local moveDir = R.myHum.MoveDirection
                if moveDir.Magnitude > 0 then
                    hrp.AssemblyLinearVelocity = V3_NEW(moveDir.X * S.bhAirSpeed * 0.95, vel.Y, moveDir.Z * S.bhAirSpeed * 0.95)
                end
            end
        else
            R.myHum.WalkSpeed = R.bhOrigSpeed
        end
    end
    
    -- FAKEDUCK (Invalid code 4 Invalid code Invalid code 3)
    if S.fdEnabled and frame % 4 == 0 and head then
        local function playAnim()
            local moving = V3_NEW(hrp.AssemblyLinearVelocity.X, 0, hrp.AssemblyLinearVelocity.Z).Magnitude > 0.5
            if moving then
                if R.fdIdleAnim and R.fdIdleAnim.IsPlaying then R.fdIdleAnim:Stop() end
                if R.fdWalkAnim and not R.fdWalkAnim.IsPlaying then R.fdWalkAnim:Play() end
            else
                if R.fdWalkAnim and R.fdWalkAnim.IsPlaying then R.fdWalkAnim:Stop() end
                if R.fdIdleAnim and not R.fdIdleAnim.IsPlaying then R.fdIdleAnim:Play() end
            end
        end
        
        if R.fdLock then
            R.fdCrouch = true
            playAnim()
        else
            RayP.FilterDescendantsInstances = {R.myChar}
            local enemy = nil
            
            for i = 1, 4 do
                local d = R.playerData[i]
                if d and not d.team and d.dist <= FD_DIST and d.head and cam then
                    local _, on = cam:WorldToScreenPoint(d.head.Position)
                    if on then
                        local res = WS:Raycast(head.Position, d.head.Position - head.Position, RayP)
                        if not res or res.Instance:IsDescendantOf(d.c) then
                            enemy = d.p
                            break
                        end
                    end
                end
            end
            
            if enemy then
                R.fdTarget, R.fdCrouch = enemy, false
                if R.fdIdleAnim and R.fdIdleAnim.IsPlaying then R.fdIdleAnim:Stop() end
                if R.fdWalkAnim and R.fdWalkAnim.IsPlaying then R.fdWalkAnim:Stop() end
            else
                if R.fdTarget then
                    local c = R.fdTarget.Character
                    if not c or not c:FindFirstChild("Humanoid") or c.Humanoid.Health <= 0 then R.fdTarget = nil end
                end
                if not R.fdTarget then R.fdCrouch = true playAnim() end
            end
        end
    end
    
    if frame > 1000 then frame = 0 end
end

-- ========== Invalid code Invalid code/Invalid code ==========
local function StartMainLoop()
    if R.mainConn then return end
    CacheChar()
    R.mainConn = RS.Heartbeat:Connect(MainLoop)
end

local function StopMainLoop()
    if R.mainConn then R.mainConn:Disconnect() R.mainConn = nil end
end

function ApplySettings()
    R.hotkeys = {}
    local needLoop = S.rbEnabled or S.fdEnabled or S.bhEnabled or S.gpEnabled or S.apEnabled
    if needLoop then StartMainLoop() else StopMainLoop() end
    if S.rbEnabled then R.hotkeys["Ragebot"] = {active = true, key = "ON"} end
    if S.fdEnabled then R.hotkeys["Fakeduck"] = {active = true, key = S.fdKey.Name} end
    if S.bhEnabled then R.hotkeys["BunnyHop"] = {active = true, key = S.bhKey.Name} end
    if S.dtEnabled then R.hotkeys["DoubleTap"] = {active = true, key = S.dtKey.Name} end
    if S.wbEnabled then R.hotkeys["Wallbang"] = {active = true, key = "ON"} end
    if S.rcEnabled then R.hotkeys["NoCollision"] = {active = true, key = "ON"} end
    if R.apActive then R.hotkeys["AI Peek v4"] = {active = true, key = S.apKey.Name} end
    if R.epActive then R.hotkeys["Exploit Pos"] = {active = true, key = S.epKey.Name} end
    if R.beActive then R.hotkeys["Barrel Ext"] = {active = true, key = S.beKey.Name .. " [" .. S.beMode .. "]"} end
    UpdateHotkeyList()
end

-- ========== CONFIG ==========
local function EnsureCfgFolder()
    if not Cfg.FS then return false end
    pcall(function() if not isfolder(Cfg.Folder) then makefolder(Cfg.Folder) end end)
    return true
end

local function RefreshCfg()
    Cfg.List = {}
    if not Cfg.FS then return end
    EnsureCfgFolder()
    pcall(function()
        for _, f in ipairs(listfiles(Cfg.Folder)) do
            local n = f:match("([^/\\]+)%.txt$")
            if n then Cfg.List[#Cfg.List+1] = n end
        end
    end)
end

local function EncodeSettings()
    local parts = {}
    for k, v in pairs(S) do
        local val
        if typeof(v) == "EnumItem" then val = "K:"..v.Name
        elseif typeof(v) == "boolean" then val = "B:"..(v and "1" or "0")
        elseif typeof(v) == "number" then val = "N:"..tostring(v)
        elseif typeof(v) == "string" then val = "S:"..v
        elseif typeof(v) == "table" then val = "T:"..HTTP:JSONEncode(v)
        elseif typeof(v) == "Color3" then val = "C:"..math.floor(v.R*255)..","..math.floor(v.G*255)..","..math.floor(v.B*255)
        else continue end
        table.insert(parts, k.."="..val)
    end
    return table.concat(parts, ";")
end

local function DecodeSettings(str)
    for pair in string.gmatch(str, "([^;]+)") do
        local k, typeVal = string.match(pair, "([^=]+)=(.+)")
        if k and typeVal and S[k] ~= nil then
            local t, v = string.match(typeVal, "^(%a):(.*)$")
            if t == "K" then pcall(function() S[k] = Enum.KeyCode[v] end)
            elseif t == "B" then S[k] = v == "1"
            elseif t == "N" then S[k] = tonumber(v) or S[k]
            elseif t == "S" then S[k] = v
            elseif t == "T" then pcall(function() S[k] = HTTP:JSONDecode(v) end)
            elseif t == "C" then
                local r, g, b = string.match(v, "(%d+),(%d+),(%d+)")
                if r and g and b then S[k] = Color3.fromRGB(tonumber(r), tonumber(g), tonumber(b)) end
            end
        end
    end
end

local function SaveCfg(name)
    if not Cfg.FS or not name or name == "" then return false end
    EnsureCfgFolder()
    local encoded = EncodeSettings()
    local ok = pcall(function() writefile(Cfg.Folder.."/"..name..".txt", encoded) end)
    if ok then RefreshCfg() end
    return ok
end

local function LoadCfg(name)
    if not Cfg.FS or not name or name == "" then return false end
    local path = Cfg.Folder.."/"..name..".txt"
    if not isfile(path) then return false end
    local ok, content = pcall(function() return readfile(path) end)
    if not ok or not content then return false end
    DecodeSettings(content)
    Cfg.Selected = name
    ApplySettings()
    return true
end

local function DeleteCfg(name)
    if not Cfg.FS or not name or name == "" then return end
    pcall(function() if isfile(Cfg.Folder.."/"..name..".txt") then delfile(Cfg.Folder.."/"..name..".txt") end end)
    RefreshCfg()
end


-- ========== HOTKEY LIST ==========
function UpdateHotkeyList()
    if not R.hkFrame then return end
    local cont = R.hkFrame:FindFirstChild("C")
    if not cont then return end
    for _, c in ipairs(cont:GetChildren()) do if c:IsA("Frame") then c:Destroy() end end
    for name, data in pairs(R.hotkeys) do
        if data.active then
            local e = Instance.new("Frame")
            e.Size, e.BackgroundTransparency, e.Parent = UDim2.new(1, 0, 0, 16), 1, cont
            local nl = Instance.new("TextLabel")
            nl.Text, nl.Size, nl.BackgroundTransparency = name, UDim2.new(0.65, 0, 1, 0), 1
            nl.TextXAlignment, nl.Font, nl.TextSize, nl.TextColor3, nl.Parent = Enum.TextXAlignment.Left, T.Font, 11, T.Text, e
            local kl = Instance.new("TextLabel")
            kl.Text, kl.Size, kl.Position = "["..data.key.."]", UDim2.new(0.35, 0, 1, 0), UDim2.new(0.65, 0, 0, 0)
            kl.BackgroundTransparency, kl.TextXAlignment, kl.Font, kl.TextSize, kl.TextColor3, kl.Parent = 1, Enum.TextXAlignment.Right, T.Font, 10, T.Dim, e
        end
    end
end

local function UpdateCfgDropdown()
    if not R.cfgDropdownList then return end
    for _, c in ipairs(R.cfgDropdownList:GetChildren()) do if c:IsA("TextButton") then c:Destroy() end end
    RefreshCfg()
    for _, cfgName in ipairs(Cfg.List) do
        local btn = Instance.new("TextButton")
        btn.Size, btn.BackgroundColor3, btn.BorderSizePixel = UDim2.new(1, 0, 0, 20), T.Dark, 0
        btn.Text, btn.TextColor3, btn.Font, btn.TextSize, btn.ZIndex = cfgName, cfgName == Cfg.Selected and T.Accent or T.Dim, T.Font, 12, 101
        btn.Parent = R.cfgDropdownList
        btn.MouseButton1Click:Connect(function()
            Cfg.Selected = cfgName
            if R.cfgDropdown then R.cfgDropdown.Text = cfgName.." ▼" end
            for _, b in ipairs(R.cfgDropdownList:GetChildren()) do
                if b:IsA("TextButton") then b.TextColor3 = b.Text == Cfg.Selected and T.Accent or T.Dim end
            end
        end)
    end
    R.cfgDropdownList.Size = UDim2.new(1, 0, 0, math.max(20, #Cfg.List * 20))
end

-- ========== VISUAL EFFECTS ==========
local function ApplyBloom()
    if R.bloomEffect then R.bloomEffect:Destroy() end
    if S.bloomEnabled then
        local bloom = Instance.new("BloomEffect")
        bloom.Intensity = S.bloomIntensity
        bloom.Size = S.bloomSize
        bloom.Threshold = S.bloomThreshold
        bloom.Parent = Light
        R.bloomEffect = bloom
    end
end

local function ApplyBlur()
    if R.blurEffect then R.blurEffect:Destroy() end
    if S.blurEnabled then
        local blur = Instance.new("BlurEffect")
        blur.Size = S.blurSize
        blur.Parent = Light
        R.blurEffect = blur
    end
end

local function ApplyColorCorrection()
    if R.colorEffect then R.colorEffect:Destroy() end
    if S.colorEnabled then
        local cc = Instance.new("ColorCorrectionEffect")
        cc.Brightness = S.ccBrightness
        cc.Contrast = S.ccContrast
        cc.Saturation = S.ccSaturation
        cc.TintColor = S.ccTintColor
        cc.Parent = Light
        R.colorEffect = cc
    end
end

local function ApplySunRays()
    if R.sunRaysEffect then R.sunRaysEffect:Destroy() end
    if S.sunRaysEnabled then
        local sr = Instance.new("SunRaysEffect")
        sr.Intensity = S.sunRaysIntensity
        sr.Spread = S.sunRaysSpread
        sr.Parent = Light
        R.sunRaysEffect = sr
    end
end

local function ApplyFog()
    if S.fogEnabled then
        Light.FogStart = S.fogStart
        Light.FogEnd = S.fogEnd
        Light.FogColor = S.fogColor
    else
        Light.FogStart = 0
        Light.FogEnd = 100000
    end
end

-- ========== DOUBLE TAP SYSTEM ==========
local DT_DIRECTIONS = {Forward = V3_NEW(0, 0, -1), Back = V3_NEW(0, 0, 1), Left = V3_NEW(-1, 0, 0), Right = V3_NEW(1, 0, 0)}

local function DT_Peek(direction)
    if not R.myHRP or not R.myChar or not R.myHum then return end
    local cam = WS.CurrentCamera
    if not cam then return end
    local myPos = R.myHRP.Position
    local origCF = R.myHRP.CFrame
    RayP.FilterDescendantsInstances = {R.myChar}
    
    if S.dtMode == "Off" or S.dtMode == "Defensive" then
        if tick() - R.dtLastPeek < 5 then return end
        local moveDir = R.myHum.MoveDirection
        if moveDir.Magnitude < 0.1 then return end
        moveDir = V3_NEW(moveDir.X, 0, moveDir.Z).Unit
        local tpPos = myPos + moveDir * S.dtDist
        if WS:Raycast(myPos, moveDir * S.dtDist, RayP) then return end
        R.myHRP.CFrame = CFrame.new(tpPos) * origCF.Rotation
        R.dtLastPeek = tick()
    else
        if tick() - R.dtLastPeek < 0.3 then return end
        local dirVec = DT_DIRECTIONS[direction]
        if not dirVec then return end
        local camCF = cam.CFrame
        local worldDir = camCF:VectorToWorldSpace(dirVec)
        worldDir = V3_NEW(worldDir.X, 0, worldDir.Z).Unit
        local peekPos = myPos + worldDir * S.dtDist
        if WS:Raycast(myPos, worldDir * S.dtDist, RayP) then return end
        R.myHRP.CFrame = CFrame.new(peekPos) * origCF.Rotation
        R.dtLastPeek = tick()
        task.wait(0.05)
        local fs = GetFireShot()
        if fs and R.myHead then
            for i = 1, 4 do
                local d = R.playerData[i]
                if d and not d.team and d.dist < 200 then
                    local tgt = S.rbHitbox == "Head" and d.head or d.torso or d.r
                    if tgt then
                        RayP.FilterDescendantsInstances = {R.myChar}
                        local res = WS:Raycast(R.myHead.Position, (tgt.Position - R.myHead.Position), RayP)
                        if not res or res.Instance:IsDescendantOf(d.c) then
                            pcall(fs.FireServer, fs, R.myHead.Position, (tgt.Position - R.myHead.Position).Unit, tgt)
                            break
                        end
                    end
                end
            end
        end
        task.wait(0.1)
        if R.myHRP and R.myHRP.Parent then R.myHRP.CFrame = origCF end
    end
end

-- ========== FAKEDUCK TOGGLE ==========
local function ToggleFakeduck()
    S.fdEnabled = not S.fdEnabled
    if S.fdEnabled then
        local hum = R.myHum
        if hum then
            local anim = hum:FindFirstChildOfClass("Animator") or Instance.new("Animator", hum)
            pcall(function()
                local a1 = Instance.new("Animation") a1.AnimationId = "rbxassetid://102226306945117"
                R.fdIdleAnim = anim:LoadAnimation(a1) R.fdIdleAnim.Priority, R.fdIdleAnim.Looped = Enum.AnimationPriority.Action4, true
                local a2 = Instance.new("Animation") a2.AnimationId = "rbxassetid://124458965304788"
                R.fdWalkAnim = anim:LoadAnimation(a2) R.fdWalkAnim.Priority, R.fdWalkAnim.Looped = Enum.AnimationPriority.Action4, true
            end)
        end
    else
        pcall(function()
            if R.fdIdleAnim and R.fdIdleAnim.IsPlaying then R.fdIdleAnim:Stop() end
            if R.fdWalkAnim and R.fdWalkAnim.IsPlaying then R.fdWalkAnim:Stop() end
        end)
        R.fdCrouch, R.fdTarget = false, nil
    end
    ApplySettings()
end

-- ========== MODELS ==========
local Models = {
    ["Mike Wazowski"] = {{}, {108792046953186}, 1, 1, false, false, nil},
    ["Fat Chicken"] = {{}, {97309274164914}, 1, 1, false, false, nil},
    ["Tung Tung Sahur"] = {{}, {117976702168543}, 1, 1, false, false, nil},
    ["Ghostface Scream"] = {{}, {109275113218599}, 1, 1, false, false, nil},
    ["Mini Pigeon"] = {{}, {134936622364544}, 1, 1, false, false, nil},
    ["Patrick Suit"] = {{}, {114561781784509}, 1, 1, false, false, nil},
    ["Buff Doge"] = {{}, {135861229178903}, 1, 1, false, false, nil},
    ["Monkey Suit"] = {{}, {133241040400728}, 1, 1, false, false, nil},
    ["Chubby Boy Meme"] = {{}, {112601613562351}, 1, 1, false, false, nil},
    ["Zelensky Skin"] = {{114906957884779}, {81828736436792}, 1, 0, false, false, CFrame.new(0, -0.3, 0)},
    ["Trump Head"] = {{77332453752069}, {}, 1, 0, false, true, nil},
    ["Putin Head"] = {{96761887509449}, {}, 1, 0, false, true, nil},
    ["Ronaldo Head"] = {{106097690556580}, {}, 1, 0, false, true, CFrame.new(0, 0.25, 0)},
    ["Dexter Morgan"] = {{83372243424051}, {}, 1, 0, false, true, nil},
    ["Kim Jong-Un"] = {{100019190392818}, {}, 1, 0, false, true, CFrame.Angles(0, math.rad(180), 0)},
    ["Messi Head"] = {{18200639690}, {}, 1, 0, false, true, nil},
    ["67 Kid"] = {{111460135975152}, {}, 1, 0, false, true, CFrame.Angles(0, math.rad(180), 0)}
}

local ModelList = {"None"}
for k in pairs(Models) do ModelList[#ModelList+1] = k end
table.sort(ModelList, function(a, b) if a == "None" then return true end if b == "None" then return false end return a < b end)

local function ApplyModel(name)
    if name == "None" or not Models[name] then return end
    local d = Models[name]
    local headIds, torsoIds, headT, bodyT, keepHead, isHeadAcc, rotation = d[1], d[2], d[3], d[4], d[5], d[6], d[7]
    
    local function onChar(char)
        task.wait(0.5)
        if not char or not char.Parent then return end
        for _, child in ipairs(char:GetChildren()) do
            if child:IsA("Accessory") then
                local isHeadAccessory = false
                local handle = child:FindFirstChild("Handle")
                if handle then
                    local weld = handle:FindFirstChildOfClass("Weld")
                    if weld and ((weld.Part0 and weld.Part0.Name == "Head") or (weld.Part1 and weld.Part1.Name == "Head")) then isHeadAccessory = true end
                end
                if not keepHead or not isHeadAccessory then child:Destroy() end
            end
        end
        for _, desc in ipairs(char:GetChildren()) do
            if desc:IsA("BasePart") and desc.Name ~= "HumanoidRootPart" then
                desc.Transparency = desc.Name == "Head" and headT or bodyT
                if desc.Name == "UpperTorso" then
                    for _, decal in ipairs(desc:GetChildren()) do if decal:IsA("Decal") then decal:Destroy() end end
                end
            end
        end
        local head = char:FindFirstChild("Head")
        if headT == 1 and head then
            local face = head:FindFirstChild("face") or head:FindFirstChild("Face")
            if face then face:Destroy() end
        end
        local torsoPart = char:FindFirstChild("UpperTorso") or char:FindFirstChild("Torso")
        for _, id in ipairs(headIds) do
            task.spawn(function()
                local ok, acc = pcall(function() return game:GetObjects("rbxassetid://"..id)[1] end)
                if ok and acc and head then
                    if isHeadAcc then
                        acc.Parent = char
                        local handle = acc:FindFirstChild("Handle")
                        if handle then
                            handle.CanCollide = false
                            local w = Instance.new("Weld")
                            w.Part0, w.Part1 = head, handle
                            w.C0 = rotation or CFrame.new(0, 0, 0)
                            w.Parent = head
                        end
                    else
                        acc.Parent = char
                        local handle = acc:FindFirstChild("Handle")
                        if handle then
                            handle.CanCollide = false
                            local w = Instance.new("Weld")
                            w.Part0, w.Part1 = head, handle
                            local baseOffset = CFrame.new(0, head.Size.Y/2, 0)
                            w.C0 = rotation and (baseOffset * rotation) or baseOffset
                            w.Parent = head
                        end
                    end
                end
            end)
        end
        if torsoPart then
            for _, id in ipairs(torsoIds) do
                task.spawn(function()
                    local ok, acc = pcall(function() return game:GetObjects("rbxassetid://"..id)[1] end)
                    if ok and acc then
                        acc.Parent = char
                        local handle = acc:FindFirstChild("Handle")
                        if handle then
                            handle.CanCollide = false
                            local w = Instance.new("Weld")
                            w.Part0, w.Part1, w.C0 = torsoPart, handle, CFrame.new(0, 0, 0)
                            w.Parent = torsoPart
                        end
                    end
                end)
            end
        end
    end
    
    if getgenv().modelConn then getgenv().modelConn:Disconnect() end
    getgenv().modelConn = LP.CharacterAdded:Connect(onChar)
    if LP.Character then onChar(LP.Character) end
end

-- ========== CUSTOM SKYBOX SYSTEM ==========
local SkyboxList = {"None", "Anime Pink", "Galaxy", "Sunset", "Night Stars", "Nebula", "Space", "Aurora", "Clouds", "Custom ID"}

local function ApplySkybox(skyboxType)
    -- Invalid code Invalid code Invalid code
    if not R.skyboxOriginal then
        for _, v in pairs(Light:GetChildren()) do
            if v:IsA("Sky") then
                R.skyboxOriginal = {
                    SkyboxBk = v.SkyboxBk, SkyboxFt = v.SkyboxFt,
                    SkyboxLf = v.SkyboxLf, SkyboxRt = v.SkyboxRt,
                    SkyboxUp = v.SkyboxUp, SkyboxDn = v.SkyboxDn
                }
                break
            end
        end
    end
    
    -- Invalid code Invalid code Invalid code
    for _, star in pairs(R.skyboxStars) do
        pcall(function() star:Destroy() end)
    end
    R.skyboxStars = {}
    
    if skyboxType == "None" then
        -- Invalid code Invalid code Invalid code
        if R.skyboxOriginal then
            local sky = Light:FindFirstChildOfClass("Sky")
            if sky then
                sky.SkyboxBk = R.skyboxOriginal.SkyboxBk
                sky.SkyboxFt = R.skyboxOriginal.SkyboxFt
                sky.SkyboxLf = R.skyboxOriginal.SkyboxLf
                sky.SkyboxRt = R.skyboxOriginal.SkyboxRt
                sky.SkyboxUp = R.skyboxOriginal.SkyboxUp
                sky.SkyboxDn = R.skyboxOriginal.SkyboxDn
            end
        end
        S.skyboxEnabled = false
        return
    end
    
    -- Invalid code Invalid code Sky Invalid code Atmosphere
    for _, v in pairs(Light:GetChildren()) do
        if v:IsA("Sky") or v:IsA("Atmosphere") then
            v:Destroy()
        end
    end
    
    -- Invalid code ID Invalid code
    local skyboxIds = {
        ["Anime Pink"] = "139989099041467",
        ["Galaxy"] = "1534951524",
        ["Sunset"] = "12064107",
        ["Night Stars"] = "6444884337",
        ["Nebula"] = "159052995",
        ["Space"] = "6071827843",
        ["Aurora"] = "6139674221",
        ["Clouds"] = "6060938146",
        ["Custom ID"] = S.skyboxId
    }
    
    local id = "http://www.roblox.com/asset/?id=" .. (skyboxIds[skyboxType] or S.skyboxId)
    
    -- Invalid code Invalid code Sky
    local sky = Instance.new("Sky", Light)
    sky.SkyboxBk = id
    sky.SkyboxFt = id
    sky.SkyboxLf = id
    sky.SkyboxRt = id
    sky.SkyboxUp = id
    sky.SkyboxDn = id
    
    Light.ClockTime = 18
    S.skyboxEnabled = true
    
    -- Invalid code Invalid code Invalid code/Invalid code Invalid code Invalid code (Invalid code)
    if S.skyboxStarsEnabled then
        local starColors = {
            Color3.fromRGB(50, 100, 255),   -- Invalid code
            Color3.fromRGB(255, 255, 50),   -- Invalid code
            Color3.fromRGB(255, 150, 50),   -- Invalid code
            Color3.fromRGB(255, 50, 50),    -- Invalid code
            Color3.fromRGB(100, 200, 255),  -- Invalid code
            Color3.fromRGB(255, 100, 200),  -- Invalid code
            Color3.fromRGB(150, 50, 255),   -- Invalid code
            Color3.fromRGB(50, 255, 150),   -- Invalid code
        }
        
        for i = 1, S.skyboxStarsCount do
            local star = Instance.new("Part")
            star.Name = "SkyboxStar_" .. i
            star.Shape = Enum.PartType.Ball
            star.Size = V3_NEW(math.random(2, 6), math.random(2, 6), math.random(2, 6))
            star.Anchored = true
            star.CanCollide = false
            star.Material = Enum.Material.Neon
            star.Color = starColors[math.random(1, #starColors)]
            star.Transparency = 0.3
            
            -- Invalid code Invalid code Invalid code Invalid code (Invalid code Invalid code Invalid code)
            local angle1 = RAD(math.random(0, 360))
            local angle2 = RAD(math.random(20, 80)) -- Invalid code Invalid code
            local dist = math.random(800, 1500)
            
            local basePos = R.myHRP and R.myHRP.Position or V3_NEW(0, 50, 0)
            star.Position = basePos + V3_NEW(
                COS(angle1) * COS(angle2) * dist,
                SIN(angle2) * dist,
                SIN(angle1) * COS(angle2) * dist
            )
            
            -- Invalid code Invalid code
            local light = Instance.new("PointLight")
            light.Color = star.Color
            light.Brightness = 2
            light.Range = 30
            light.Parent = star
            
            star.Parent = WS
            table.insert(R.skyboxStars, star)
        end
        
        -- Invalid code Invalid code Invalid code (Invalid code - Invalid code Heartbeat Invalid code Invalid code Invalid code)
        local starConn
        local starIndex = 1
        starConn = RS.Heartbeat:Connect(function()
            if not S.skyboxEnabled or #R.skyboxStars == 0 then
                if starConn then starConn:Disconnect() end
                return
            end
            -- Invalid code Invalid code 2 Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
            for _ = 1, 2 do
                local star = R.skyboxStars[starIndex]
                if star and star.Parent then
                    local light = star:FindFirstChildOfClass("PointLight")
                    if light then
                        light.Brightness = 1.5 + math.random() * 1.5
                    end
                    star.Transparency = 0.2 + math.random() * 0.3
                end
                starIndex = starIndex + 1
                if starIndex > #R.skyboxStars then starIndex = 1 end
            end
        end)
    end
    
    -- print("Custom Skybox Applied: " .. skyboxType)
end

local function RemoveSkybox()
    ApplySkybox("None")
end


-- ========== UI LIBRARY ==========
local Lib = {}
function Lib:Create(title)
    if R.gui then R.gui:Destroy() end
    R.gui = Instance.new("ScreenGui")
    R.gui.Name, R.gui.ResetOnSpawn, R.gui.Parent = "Arc", false, game:GetService("CoreGui")
    
    -- Invalid code Hit Logger Invalid code Invalid code GUI
    CreateHitLogger()
    
    -- Invalid code Invalid code Invalid code
    SetupKillTracking()
    
    -- Create HK Frame
    local hkf = Instance.new("Frame")
    hkf.Name, hkf.Size, hkf.Position = "HK", UDim2.new(0, 160, 0, 24), UDim2.new(1, -170, 0, 200)
    hkf.BackgroundColor3, hkf.BackgroundTransparency, hkf.BorderSizePixel, hkf.Active = T.Main, 0.1, 0, true
    hkf.Parent = R.gui
    R.hkFrame = hkf
    Instance.new("UIStroke", hkf).Color = T.Stroke
    local gl = Instance.new("Frame") gl.Size, gl.BorderSizePixel, gl.Parent = UDim2.new(1, 0, 0, 2), 0, hkf
    Instance.new("UIGradient", gl).Color = ColorSequence.new({ColorSequenceKeypoint.new(0, T.Accent), ColorSequenceKeypoint.new(1, Color3.fromRGB(100, 200, 50))})
    local hktitle = Instance.new("TextLabel")
    hktitle.Text, hktitle.Size, hktitle.Position, hktitle.BackgroundTransparency = "hotkeys", UDim2.new(1, 0, 0, 20), UDim2.new(0, 0, 0, 3), 1
    hktitle.Font, hktitle.TextSize, hktitle.TextColor3, hktitle.Parent = T.Font, 11, T.Dim, hkf
    local hkcont = Instance.new("Frame")
    hkcont.Name, hkcont.Size, hkcont.Position, hkcont.BackgroundTransparency = "C", UDim2.new(1, -8, 1, -24), UDim2.new(0, 4, 0, 22), 1
    hkcont.Parent = hkf
    local hklist = Instance.new("UIListLayout") hklist.Padding, hklist.Parent = UDim.new(0, 2), hkcont
    hklist:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function() hkf.Size = UDim2.new(0, 160, 0, math.max(24, hklist.AbsoluteContentSize.Y + 26)) end)
    local hkdrag, hkdStart, hkdPos = false, nil, nil
    hkf.InputBegan:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 then hkdrag, hkdStart, hkdPos = true, i.Position, hkf.Position end end)
    hkf.InputChanged:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseMovement and hkdrag then local d = i.Position - hkdStart hkf.Position = UDim2.new(hkdPos.X.Scale, hkdPos.X.Offset + d.X, hkdPos.Y.Scale, hkdPos.Y.Offset + d.Y) end end)
    table.insert(R.conns, UIS.InputEnded:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 then hkdrag = false end end))
    
    -- Create WM Frame
    local wmf = Instance.new("Frame")
    wmf.Size, wmf.Position = UDim2.new(0, 200, 0, 22), UDim2.new(0, 10, 0, 10)
    wmf.BackgroundColor3, wmf.BackgroundTransparency, wmf.BorderSizePixel = T.Main, 0.1, 0
    wmf.Parent = R.gui
    R.wmFrame = wmf
    Instance.new("UIStroke", wmf).Color = T.Stroke
    local wmgl = Instance.new("Frame") wmgl.Size, wmgl.BorderSizePixel, wmgl.Parent = UDim2.new(1, 0, 0, 2), 0, wmf
    Instance.new("UIGradient", wmgl).Color = ColorSequence.new({ColorSequenceKeypoint.new(0, T.Accent), ColorSequenceKeypoint.new(1, Color3.fromRGB(100, 200, 50))})
    local wmtxt = Instance.new("TextLabel")
    wmtxt.Name, wmtxt.Size, wmtxt.Position = "T", UDim2.new(1, -10, 1, -2), UDim2.new(0, 5, 0, 2)
    wmtxt.BackgroundTransparency, wmtxt.Font, wmtxt.TextSize, wmtxt.TextColor3, wmtxt.TextXAlignment = 1, T.Font, 11, T.Text, Enum.TextXAlignment.Left
    wmtxt.Text, wmtxt.Parent = "Arcanum.lua | loading...", wmf
    -- Invalid code Invalid code FPS Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
    task.delay(2, function()
        local lastTime, frameCount, fps = tick(), 0, 60
        local fpsConn = RS.Heartbeat:Connect(function() frameCount = frameCount + 1 end)
        while R.running and wmf and wmf.Parent do
            local now = tick()
            local elapsed = now - lastTime
            if elapsed >= 1 then fps = math.floor(frameCount / elapsed) frameCount = 0 lastTime = now end
            local ping = FLOOR(LP:GetNetworkPing() * 1000)
            wmtxt.Text = "Arcanum.lua | fps: "..fps.." | ping: "..ping.."ms"
            wmf.Size = UDim2.new(0, TS:GetTextSize(wmtxt.Text, 11, T.Font, Vector2.new(1000, 22)).X + 20, 0, 22)
            task.wait(1) -- Invalid code Invalid code Invalid code Invalid code Invalid code 0.5
        end
        if fpsConn then fpsConn:Disconnect() end
    end)
    
    -- Create Time Frame
    local tmf = Instance.new("Frame")
    tmf.Size, tmf.Position = UDim2.new(0, 80, 0, 22), UDim2.new(1, -90, 0, 10)
    tmf.BackgroundColor3, tmf.BackgroundTransparency, tmf.BorderSizePixel, tmf.Active = T.Main, 0.1, 0, true
    tmf.Parent = R.gui
    R.timeFrame = tmf
    Instance.new("UIStroke", tmf).Color = T.Stroke
    local tmgl = Instance.new("Frame") tmgl.Size, tmgl.BorderSizePixel, tmgl.Parent = UDim2.new(1, 0, 0, 2), 0, tmf
    Instance.new("UIGradient", tmgl).Color = ColorSequence.new({ColorSequenceKeypoint.new(0, T.Accent), ColorSequenceKeypoint.new(1, Color3.fromRGB(100, 200, 50))})
    local tmtxt = Instance.new("TextLabel")
    tmtxt.Name, tmtxt.Size, tmtxt.Position = "T", UDim2.new(1, -10, 1, -2), UDim2.new(0, 5, 0, 2)
    tmtxt.BackgroundTransparency, tmtxt.Font, tmtxt.TextSize, tmtxt.TextColor3, tmtxt.TextXAlignment = 1, T.Font, 11, T.Text, Enum.TextXAlignment.Center
    tmtxt.Text, tmtxt.Parent = "--:--:--", tmf
    local tmdrag, tmdStart, tmdPos = false, nil, nil
    tmf.InputBegan:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 then tmdrag, tmdStart, tmdPos = true, i.Position, tmf.Position end end)
    tmf.InputChanged:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseMovement and tmdrag then local d = i.Position - tmdStart tmf.Position = UDim2.new(tmdPos.X.Scale, tmdPos.X.Offset + d.X, tmdPos.Y.Scale, tmdPos.Y.Offset + d.Y) end end)
    table.insert(R.conns, UIS.InputEnded:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 then tmdrag = false end end))
    task.spawn(function() while R.running and tmf and tmf.Parent do local bt = GetBerlinTime() tmtxt.Text = string.format("%02d:%02d:%02d", bt.hour, bt.min, bt.sec) task.wait(1) end end)
    
    -- Main Window
    local W = Instance.new("Frame")
    W.Name, W.Size, W.Position, W.BackgroundColor3, W.BorderSizePixel = "M", UDim2.new(0, 620, 0, 450), UDim2.new(0.5, -310, 0.5, -225), T.Main, 0
    W.Parent = R.gui
    R.main = W
    Instance.new("UIStroke", W).Color = T.Border
    local wgl = Instance.new("Frame") wgl.Size, wgl.Position, wgl.BorderSizePixel = UDim2.new(1, 0, 0, 2), UDim2.new(0, 0, 0, 0), 0 wgl.Parent = W
    local wug = Instance.new("UIGradient")
    wug.Color = ColorSequence.new({ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 0)), ColorSequenceKeypoint.new(0.2, Color3.fromRGB(255, 255, 0)), ColorSequenceKeypoint.new(0.4, Color3.fromRGB(0, 255, 0)), ColorSequenceKeypoint.new(0.6, Color3.fromRGB(0, 255, 255)), ColorSequenceKeypoint.new(0.8, Color3.fromRGB(0, 0, 255)), ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 0, 255))})
    wug.Parent = wgl
    local tl = Instance.new("TextLabel")
    tl.Text, tl.Size, tl.Position, tl.BackgroundTransparency = title, UDim2.new(1, 0, 0, 20), UDim2.new(0, 0, 0, 4), 1
    tl.Font, tl.TextSize, tl.TextColor3, tl.Parent = T.Font, 13, T.Text, W
    local tc = Instance.new("Frame")
    tc.Size, tc.Position, tc.BackgroundColor3, tc.BorderColor3 = UDim2.new(1, -20, 0, 40), UDim2.new(0, 10, 0, 30), T.Group, T.Stroke
    tc.Parent = W
    local tlay = Instance.new("UIListLayout")
    tlay.FillDirection, tlay.HorizontalAlignment, tlay.VerticalAlignment, tlay.Padding = Enum.FillDirection.Horizontal, Enum.HorizontalAlignment.Center, Enum.VerticalAlignment.Center, UDim.new(0, 15)
    tlay.Parent = tc
    local pc = Instance.new("Frame")
    pc.Size, pc.Position, pc.BackgroundTransparency = UDim2.new(1, -20, 1, -85), UDim2.new(0, 10, 0, 75), 1
    pc.Parent = W
    
    W.InputBegan:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 and i.Position.Y < W.AbsolutePosition.Y + 30 then R.drag, R.dragStart, R.startPos = true, i.Position, W.Position end end)
    W.InputChanged:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseMovement and R.drag then local d = i.Position - R.dragStart W.Position = UDim2.new(R.startPos.X.Scale, R.startPos.X.Offset + d.X, R.startPos.Y.Scale, R.startPos.Y.Offset + d.Y) end end)
    table.insert(R.conns, UIS.InputChanged:Connect(function(i)
        if R.sliderDrag and i.UserInputType == Enum.UserInputType.MouseMovement then
            local d = R.sliderDrag
            local rel = CLAMP((i.Position.X - d.bg.AbsolutePosition.X) / d.bg.AbsoluteSize.X, 0, 1)
            d.val = FLOOR(d.min + (d.max - d.min) * rel)
            d.fill.Size = UDim2.new(rel, 0, 1, 0)
            d.lbl.Text = d.text..": "..d.val
            if d.cb then d.cb(d.val) end
        end
    end))
    table.insert(R.conns, UIS.InputEnded:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 then R.drag, R.sliderDrag = false, nil end end))
    
    local Tabs = {}
    local tabButtons = {}
    function Tabs:NewTab(name)
        local btn = Instance.new("TextButton")
        btn.Text, btn.Size, btn.BackgroundTransparency, btn.Font, btn.TextSize, btn.TextColor3 = name, UDim2.new(0, 70, 1, 0), 1, T.Font, 13, T.Dim
        btn.Parent = tc
        table.insert(tabButtons, btn)
        local page = Instance.new("ScrollingFrame")
        page.Size, page.BackgroundTransparency, page.Visible, page.BorderSizePixel = UDim2.new(1, 0, 1, 0), 1, false, 0
        page.ScrollBarThickness, page.ScrollBarImageColor3, page.ScrollingDirection = 4, T.Accent, Enum.ScrollingDirection.Y
        page.CanvasSize, page.AutomaticCanvasSize = UDim2.new(0, 0, 0, 0), Enum.AutomaticSize.Y
        page.Parent = pc
        local lc = Instance.new("Frame") lc.Size, lc.BackgroundTransparency, lc.AutomaticSize = UDim2.new(0.48, 0, 0, 0), 1, Enum.AutomaticSize.Y lc.Parent = page
        Instance.new("UIListLayout", lc).Padding = UDim.new(0, 12)
        local rc = Instance.new("Frame") rc.Size, rc.Position, rc.BackgroundTransparency, rc.AutomaticSize = UDim2.new(0.48, 0, 0, 0), UDim2.new(0.52, 0, 0, 0), 1, Enum.AutomaticSize.Y rc.Parent = page
        Instance.new("UIListLayout", rc).Padding = UDim.new(0, 12)
        btn.MouseButton1Click:Connect(function()
            for _, p in ipairs(pc:GetChildren()) do p.Visible = false end
            for _, t in ipairs(tc:GetChildren()) do if t:IsA("TextButton") then t.TextColor3 = T.Dim end end
            page.Visible, btn.TextColor3 = true, T.Accent
        end)
        if #tc:GetChildren() == 2 then page.Visible, btn.TextColor3 = true, T.Accent end
        -- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
        table.insert(R.accentElements, {type = "tab", btn = btn, page = page, allTabs = tabButtons})
        
        local TL = {}
        function TL:NewGroupbox(side, gtitle)
            local col = side == "Right" and rc or lc
            local grp = Instance.new("Frame") grp.Size, grp.BackgroundTransparency, grp.AutomaticSize = UDim2.new(1, 0, 0, 0), 1, Enum.AutomaticSize.Y grp.Parent = col
            local brd = Instance.new("Frame") brd.Size, brd.Position, brd.BackgroundColor3, brd.BorderColor3, brd.AutomaticSize = UDim2.new(1, 0, 0, 0), UDim2.new(0, 0, 0, 8), T.Main, T.Stroke, Enum.AutomaticSize.Y brd.Parent = grp
            local ttl = Instance.new("TextLabel") ttl.Text, ttl.Position, ttl.AutomaticSize, ttl.BackgroundColor3, ttl.BorderSizePixel = gtitle, UDim2.new(0, 12, 0, 14), Enum.AutomaticSize.X, T.Main, 0 ttl.TextColor3, ttl.Font, ttl.TextSize, ttl.ZIndex = T.Text, T.Font, 12, 2 ttl.Parent = grp
            local cnt = Instance.new("Frame") cnt.Size, cnt.Position, cnt.BackgroundTransparency, cnt.AutomaticSize = UDim2.new(1, -16, 0, 0), UDim2.new(0, 8, 0, 22), 1, Enum.AutomaticSize.Y cnt.Parent = brd
            Instance.new("UIListLayout", cnt).Padding = UDim.new(0, 8)
            Instance.new("UIPadding", brd).PaddingBottom = UDim.new(0, 8)
            
            local G = {}
            function G:Toggle(text, def, cb)
                local f = Instance.new("Frame") f.Size, f.BackgroundTransparency, f.Parent = UDim2.new(1, 0, 0, 20), 1, cnt
                local box = Instance.new("Frame") box.Size, box.Position, box.BackgroundColor3, box.Parent = UDim2.new(0, 16, 0, 16), UDim2.new(0, 0, 0, 2), T.Dark, f
                Instance.new("UIStroke", box).Color = T.Stroke
                local lbl = Instance.new("TextLabel") lbl.Text, lbl.Size, lbl.Position, lbl.BackgroundTransparency = text, UDim2.new(1, -25, 1, 0), UDim2.new(0, 25, 0, 0), 1 lbl.TextXAlignment, lbl.TextColor3, lbl.Font, lbl.TextSize, lbl.Parent = Enum.TextXAlignment.Left, T.Dim, T.Font, 13, f
                local b = Instance.new("TextButton") b.Size, b.BackgroundTransparency, b.Text, b.Parent = UDim2.new(1, 0, 1, 0), 1, "", f
                local en = def
                local function upd() box.BackgroundColor3 = en and T.Accent or T.Dark lbl.TextColor3 = en and T.Text or T.Dim if cb then cb(en) end end
                b.MouseButton1Click:Connect(function() en = not en upd() end)
                upd()
                -- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
                table.insert(R.accentElements, {type = "toggle", box = box, getEnabled = function() return en end})
                return G
            end
            function G:Slider(text, min, max, def, cb)
                local f = Instance.new("Frame") f.Size, f.BackgroundTransparency, f.Parent = UDim2.new(1, 0, 0, 35), 1, cnt
                local lbl = Instance.new("TextLabel") lbl.Text, lbl.Size, lbl.BackgroundTransparency = text..": "..def, UDim2.new(1, 0, 0, 15), 1 lbl.TextXAlignment, lbl.TextColor3, lbl.Font, lbl.TextSize, lbl.Parent = Enum.TextXAlignment.Left, T.Dim, T.Font, 13, f
                local bg = Instance.new("Frame") bg.Size, bg.Position, bg.BackgroundColor3, bg.BorderColor3, bg.Parent = UDim2.new(1, 0, 0, 12), UDim2.new(0, 0, 0, 18), T.Dark, T.Stroke, f
                local fill = Instance.new("Frame") fill.Size, fill.BackgroundColor3, fill.BorderSizePixel, fill.Parent = UDim2.new((def-min)/(max-min), 0, 1, 0), T.Accent, 0, bg
                local data = {bg = bg, fill = fill, lbl = lbl, text = text, min = min, max = max, val = def, cb = cb}
                bg.InputBegan:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 then R.sliderDrag = data end end)
                -- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
                table.insert(R.accentElements, {type = "slider", fill = fill})
                return G
            end
            function G:Dropdown(text, opts, def, cb)
                local f = Instance.new("Frame") f.Size, f.BackgroundTransparency, f.ClipsDescendants, f.ZIndex, f.Parent = UDim2.new(1, 0, 0, 40), 1, false, 10, cnt
                local lbl = Instance.new("TextLabel") lbl.Text, lbl.Size, lbl.BackgroundTransparency = text, UDim2.new(1, 0, 0, 15), 1 lbl.TextXAlignment, lbl.TextColor3, lbl.Font, lbl.TextSize, lbl.Parent = Enum.TextXAlignment.Left, T.Dim, T.Font, 13, f
                local box = Instance.new("TextButton") box.Size, box.Position, box.BackgroundColor3, box.BorderColor3 = UDim2.new(1, 0, 0, 20), UDim2.new(0, 0, 0, 18), T.Dark, T.Stroke box.Text, box.TextColor3, box.Font, box.TextSize, box.Parent = def.." ▼", T.Text, T.Font, 13, f
                local ol = Instance.new("Frame") ol.Size, ol.Position, ol.BackgroundColor3, ol.BorderColor3, ol.Visible, ol.ZIndex = UDim2.new(1, 0, 0, #opts * 20), UDim2.new(0, 0, 0, 38), T.Darker, T.Stroke, false, 100 ol.Parent = f
                Instance.new("UIListLayout", ol)
                local cur, open = def, false
                local optButtons = {}
                for _, opt in ipairs(opts) do
                    local ob = Instance.new("TextButton") ob.Size, ob.BackgroundColor3, ob.BorderSizePixel = UDim2.new(1, 0, 0, 20), T.Dark, 0 ob.Text, ob.TextColor3, ob.Font, ob.TextSize, ob.ZIndex, ob.Parent = opt, opt == cur and T.Accent or T.Dim, T.Font, 12, 101, ol
                    table.insert(optButtons, ob)
                    ob.MouseButton1Click:Connect(function() cur, box.Text, ol.Visible, open = opt, opt.." ▼", false, false f.Size = UDim2.new(1, 0, 0, 40) for _, b in ipairs(ol:GetChildren()) do if b:IsA("TextButton") then b.TextColor3 = b.Text == cur and T.Accent or T.Dim end end if cb then cb(cur) end end)
                end
                box.MouseButton1Click:Connect(function() open = not open ol.Visible = open f.Size = open and UDim2.new(1, 0, 0, 40 + #opts * 20) or UDim2.new(1, 0, 0, 40) end)
                -- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
                table.insert(R.accentElements, {type = "dropdown", buttons = optButtons, getCurrent = function() return cur end})
                return G
            end
            function G:Button(text, cb)
                local b = Instance.new("TextButton") b.Size, b.BackgroundColor3, b.BorderColor3 = UDim2.new(1, 0, 0, 22), T.Dark, T.Stroke b.Text, b.TextColor3, b.Font, b.TextSize, b.Parent = text, T.Text, T.Font, 13, cnt
                b.MouseButton1Click:Connect(cb)
                return G
            end
            function G:Keybind(text, def, cb)
                local f = Instance.new("Frame") f.Size, f.BackgroundTransparency, f.Parent = UDim2.new(1, 0, 0, 20), 1, cnt
                local lbl = Instance.new("TextLabel") lbl.Text, lbl.Size, lbl.BackgroundTransparency = text, UDim2.new(0.6, 0, 1, 0), 1 lbl.TextXAlignment, lbl.TextColor3, lbl.Font, lbl.TextSize, lbl.Parent = Enum.TextXAlignment.Left, T.Dim, T.Font, 13, f
                local b = Instance.new("TextButton") b.Size, b.Position, b.BackgroundColor3, b.BorderColor3 = UDim2.new(0.3, 0, 1, 0), UDim2.new(0.7, 0, 0, 0), Color3.fromRGB(22,22,22), T.Stroke b.Text, b.TextColor3, b.Font, b.TextSize, b.Parent = "["..def.Name.."]", T.Dim, T.Font, 11, f
                local waiting = false
                b.MouseButton1Click:Connect(function() waiting, b.Text, b.TextColor3 = true, "[...]", T.Accent end)
                table.insert(R.conns, UIS.InputBegan:Connect(function(i) if waiting and i.UserInputType == Enum.UserInputType.Keyboard then waiting, b.Text, b.TextColor3 = false, "["..i.KeyCode.Name.."]", T.Dim if cb then cb(i.KeyCode) end end end))
                return G
            end
            function G:TextBox(text, def, cb)
                local f = Instance.new("Frame") f.Size, f.BackgroundTransparency, f.Parent = UDim2.new(1, 0, 0, 40), 1, cnt
                local lbl = Instance.new("TextLabel") lbl.Text, lbl.Size, lbl.BackgroundTransparency = text, UDim2.new(1, 0, 0, 15), 1 lbl.TextXAlignment, lbl.TextColor3, lbl.Font, lbl.TextSize, lbl.Parent = Enum.TextXAlignment.Left, T.Dim, T.Font, 13, f
                local tb = Instance.new("TextBox") tb.Size, tb.Position, tb.BackgroundColor3, tb.BorderColor3 = UDim2.new(1, 0, 0, 20), UDim2.new(0, 0, 0, 18), T.Dark, T.Stroke tb.Text, tb.PlaceholderText, tb.TextColor3, tb.Font, tb.TextSize, tb.ClearTextOnFocus, tb.Parent = def or "", "Enter...", T.Text, T.Font, 12, false, f
                tb.FocusLost:Connect(function() if cb then cb(tb.Text) end end)
                return G
            end
            function G:ConfigDropdown(text)
                local f = Instance.new("Frame") f.Size, f.BackgroundTransparency, f.ClipsDescendants, f.ZIndex, f.Parent = UDim2.new(1, 0, 0, 40), 1, false, 10, cnt
                local lbl = Instance.new("TextLabel") lbl.Text, lbl.Size, lbl.BackgroundTransparency = text, UDim2.new(1, 0, 0, 15), 1 lbl.TextXAlignment, lbl.TextColor3, lbl.Font, lbl.TextSize, lbl.Parent = Enum.TextXAlignment.Left, T.Dim, T.Font, 13, f
                local box = Instance.new("TextButton") box.Size, box.Position, box.BackgroundColor3, box.BorderColor3 = UDim2.new(1, 0, 0, 20), UDim2.new(0, 0, 0, 18), T.Dark, T.Stroke box.Text, box.TextColor3, box.Font, box.TextSize, box.Parent = (Cfg.Selected ~= "" and Cfg.Selected or "Select...").." ▼", T.Text, T.Font, 13, f
                R.cfgDropdown = box
                local ol = Instance.new("Frame") ol.Size, ol.Position, ol.BackgroundColor3, ol.BorderColor3, ol.Visible, ol.ZIndex = UDim2.new(1, 0, 0, 20), UDim2.new(0, 0, 0, 38), T.Darker, T.Stroke, false, 100 ol.Parent = f
                Instance.new("UIListLayout", ol)
                R.cfgDropdownList = ol
                local open = false
                box.MouseButton1Click:Connect(function() open = not open ol.Visible = open if open then UpdateCfgDropdown() f.Size = UDim2.new(1, 0, 0, 40 + math.max(20, #Cfg.List * 20)) else f.Size = UDim2.new(1, 0, 0, 40) end end)
                UpdateCfgDropdown()
                return G
            end
            return G
        end
        return TL
    end
    return Tabs
end


-- ========== BUILD UI ==========
local Win = Lib:Create("lvzz crack https://www.youtube.com/@Script_house/videos")

-- RAGE
do
    local Tab = Win:NewTab("Rage")
    local rb = Tab:NewGroupbox("Left", "Ragebot")
    rb:Toggle("Enable", false, function(v) S.rbEnabled = v ApplySettings() end)
    rb:Toggle("Resolver", false, function(v) S.rbResolver = v end)
    rb:Dropdown("Resolver Mode", {"Safe","Aggressive"}, "Safe", function(v) S.rbResolverMode = v end)
    rb:Toggle("Prediction", true, function(v) S.rbPrediction = v end)
    rb:Toggle("Auto Fire", true, function(v) S.rbAutoFire = v end)
    rb:Toggle("Team Check", true, function(v) S.rbTeamCheck = v end)
    rb:Dropdown("Hitbox", {"Head","Torso"}, "Head", function(v) S.rbHitbox = v end)
    rb:Toggle("Wall Check", true, function(v) S.rbWallCheck = v end)
    rb:Toggle("No Air Shot", true, function(v) S.rbNoAir = v end)
    rb:Toggle("Smart Aim", true, function(v) S.rbSmartAim = v end)
    rb:Toggle("Air Shoot", false, function(v) S.rbAirShoot = v end)
    rb:Dropdown("Prediction", {"Default", "Beta AI"}, "Default", function(v) S.rbPredMode = v end)
    rb:Slider("BodyAim HP", 10, 100, 50, function(v) S.rbBodyAimHP = v end)
    
    -- BETA AI SETTINGS
    local ai = Tab:NewGroupbox("Left", "Beta AI Settings")
    ai:Slider("Confidence %", 30, 100, 60, function(v) S.aiConfThreshold = v end)
    ai:Slider("History Size", 10, 50, 30, function(v) S.aiHistorySize = v AI_PRED.HISTORY_SIZE = v end)
    ai:Toggle("Peek Detect", true, function(v) S.aiPeekDetect = v AI_PRED.peekDetect = v end)
    ai:Toggle("Strafe Detect", true, function(v) S.aiStrafeDetect = v AI_PRED.strafeDetect = v end)
    ai:Toggle("Visual Box", false, function(v) S.aiVisualBox = v if not v then AI_ClearVisuals() end end)
    ai:Toggle("Visual Trace", false, function(v) S.aiVisualTrace = v if not v then AI_ClearVisuals() end end)
    
    -- DOUBLE TAP
    local dt = Tab:NewGroupbox("Right", "Double Tap")
    dt:Toggle("Enable", false, function(v) 
        S.dtEnabled = v 
        ApplySettings()
    end)
    dt:Keybind("Key", Enum.KeyCode.E, function(k) S.dtKey = k end)
    dt:Slider("TP Distance", 3, 10, 6, function(v) S.dtDist = v end)
    
    -- AI PEEK V4 GROUP
    local ap = Tab:NewGroupbox("Left", "AI Peek v4")
    ap:Toggle("Enable", false, function(v) 
        S.apEnabled = v 
        if not v and R.apActive then
            AP_Disable()
        end
    end)
    ap:Keybind("Key", Enum.KeyCode.LeftAlt, function(k) S.apKey = k end)
    ap:Dropdown("Mode", {"Hold","Toggle"}, "Hold", function(v) S.apMode = v end)
    ap:Toggle("Show Points", false, function(v) 
        S.apShowPoints = v 
        if R.apActive then
            AP_RemovePoints()
            AP_CreatePoints()
        end
    end)
    ap:Toggle("ESP Outline", false, function(v) 
        S.apESP = v 
        if R.apActive then AP_UpdateESP() end
    end)
    ap:Toggle("Team Check", true, function(v) S.apTeamCheck = v end)
    ap:Slider("Cooldown (ms)", 0, 3000, FLOOR(S.apCooldown * 1000), function(v) S.apCooldown = v / 1000 end)
    ap:Slider("Range", 0, 200, S.apRange, function(v) S.apRange = v end)
    ap:Slider("Peek Distance", 0, 20, S.apPeekDist, function(v) 
        S.apPeekDist = v 
        if R.apActive then AP_RemovePoints() AP_CreatePoints() end
    end)
    ap:Slider("Peek Speed", 0, 1000, S.apSpeed, function(v) S.apSpeed = v end)
    ap:Slider("Max Height", 1, 10, FLOOR(S.apHeight), function(v) S.apHeight = v end)
    
    -- GHOST PEEK GROUP
    local gp = Tab:NewGroupbox("Right", "Ghost Peek")
    gp:Toggle("Enable", true, function(v) 
        S.gpEnabled = v 
        if not v and R.gpActive then
            GP_Disable()
        end
    end)
    gp:Keybind("Key", Enum.KeyCode.Q, function(k) S.gpKey = k end)
    gp:Dropdown("Mode", {"Hold","Toggle"}, "Hold", function(v) S.gpMode = v end)
    gp:Toggle("Auto Shoot", true, function(v) S.gpAutoshoot = v end)
    gp:Toggle("Team Check", true, function(v) S.gpTeamCheck = v end)
    gp:Slider("Range", 20, 300, S.gpRange, function(v) S.gpRange = v end)
    gp:Slider("Peek Distance", 5, 60, S.gpPeekDist, function(v) S.gpPeekDist = v end)
    gp:Slider("Max Height", 1, 15, S.gpHeight, function(v) S.gpHeight = v end)
    gp:Slider("Quality", 0, 100, S.gpQuality, function(v) S.gpQuality = v end)
end

-- AA
do
    local Tab = Win:NewTab("AA")
    local fd = Tab:NewGroupbox("Left", "Fakeduck")
    fd:Toggle("Enable", false, function(v) if v ~= S.fdEnabled then ToggleFakeduck() end end)
    fd:Toggle("Team Check", true, function(v) S.fdTeamCheck = v end)
    fd:Keybind("Key", Enum.KeyCode.X, function(k) S.fdKey = k end)
    fd:Keybind("Lock Key", Enum.KeyCode.V, function(k) S.fdLockKey = k end)
end

-- VISUALS
do
    local Tab = Win:NewTab("Visuals")
    
    -- LOCAL PLAYER
    local lp = Tab:NewGroupbox("Left", "Local Player")
    lp:Toggle("Watermark", true, function(v) S.wmVisible = v if R.wmFrame then R.wmFrame.Visible = v end end)
    lp:Toggle("Hotkey List", true, function(v) S.hkVisible = v if R.hkFrame then R.hkFrame.Visible = v end end)
    lp:Toggle("Time", true, function(v) S.timeVisible = v if R.timeFrame then R.timeFrame.Visible = v end end)
    
    -- HIT LOGGER
    local hl = Tab:NewGroupbox("Left", "Hit Logger")
    hl:Toggle("Enable", true, function(v) S.hlEnabled = v if R.hlFrame then R.hlFrame.Visible = v end end)
    hl:Slider("Max Logs", 4, 15, 8, function(v) S.hlMaxLogs = v end)
    
    -- AIMVIEW
    local av = Tab:NewGroupbox("Left", "AimView")
    av:Toggle("Enable", false, function(v) 
        S.avEnabled = v 
        if not v then AV_RemoveLine() end
    end)
    av:Dropdown("Color", {"Red","Green","Blue","White","Yellow","Purple","Cyan"}, "Red", function(v)
        local colors = {Red = Color3.fromRGB(255, 0, 0), Green = Color3.fromRGB(168, 247, 50), Blue = Color3.fromRGB(50, 100, 255), White = Color3.fromRGB(255, 255, 255), Yellow = Color3.fromRGB(255, 255, 0), Purple = Color3.fromRGB(200, 50, 255), Cyan = Color3.fromRGB(0, 255, 255)}
        S.avColor = colors[v] or Color3.fromRGB(255, 0, 0)
        if R.avLine then R.avLine.Color = S.avColor end
    end)
    av:Slider("Transparency", 0, 100, 30, function(v) 
        S.avTransparency = v / 100
        if R.avLine then R.avLine.Transparency = S.avTransparency end
    end)
    
    -- WORLD
    local wv = Tab:NewGroupbox("Left", "World")
    wv:Toggle("Hitmarker", true, function(v) S.hmEnabled = v end)
    wv:Dropdown("HM Color", {"Green","Red","Blue","White","Yellow","Purple","Cyan"}, "Green", function(v)
        local colors = {Green = Color3.fromRGB(168, 247, 50), Red = Color3.fromRGB(255, 50, 50), Blue = Color3.fromRGB(50, 100, 255), White = Color3.fromRGB(255, 255, 255), Yellow = Color3.fromRGB(255, 255, 0), Purple = Color3.fromRGB(200, 50, 255), Cyan = Color3.fromRGB(0, 255, 255)}
        S.hmColor = colors[v] or Color3.fromRGB(168, 247, 50)
    end)
    wv:Toggle("Kill Effect", true, function(v) S.keEnabled = v end)
    wv:Dropdown("KE Color", {"White","Red","Blue","Green","Yellow","Purple","Cyan"}, "White", function(v)
        local colors = {White = Color3.fromRGB(255, 255, 255), Red = Color3.fromRGB(255, 50, 50), Blue = Color3.fromRGB(50, 100, 255), Green = Color3.fromRGB(168, 247, 50), Yellow = Color3.fromRGB(255, 255, 0), Purple = Color3.fromRGB(200, 50, 255), Cyan = Color3.fromRGB(0, 255, 255)}
        S.keColor = colors[v] or Color3.fromRGB(255, 255, 255)
    end)
    wv:Toggle("Fortnite Damage", true, function(v) S.fdmgEnabled = v end)
    wv:Dropdown("FD Color", {"White","Red","Blue","Green","Yellow","Purple","Cyan"}, "White", function(v)
        local colors = {White = Color3.fromRGB(255, 255, 255), Red = Color3.fromRGB(255, 50, 50), Blue = Color3.fromRGB(50, 100, 255), Green = Color3.fromRGB(168, 247, 50), Yellow = Color3.fromRGB(255, 255, 0), Purple = Color3.fromRGB(200, 50, 255), Cyan = Color3.fromRGB(0, 255, 255)}
        S.fdmgColor = colors[v] or Color3.fromRGB(255, 255, 255)
    end)
    
    -- CUSTOM HIT SOUND
    local hs = Tab:NewGroupbox("Left", "Hit Sound")
    hs:Toggle("Enable", false, function(v) S.hsEnabled = v end)
    hs:Dropdown("Sound", HitSoundsList, "Default", function(v) 
        S.hsSelected = v 
        if v ~= "Custom" then
            S.hsSoundId = HitSounds[v] or HitSounds["Default"]
        end
    end)
    hs:TextBox("Custom ID", "", function(v) 
        if S.hsSelected == "Custom" and v ~= "" then 
            S.hsSoundId = v 
        end 
    end)
    hs:Slider("Volume", 0, 200, 100, function(v) S.hsVolume = v end)
    hs:Button("Test Sound", function() PlayHitSound() end)
    
    -- EFFECTS
    local vfx = Tab:NewGroupbox("Right", "Effects")
    vfx:Toggle("Bloom", false, function(v) S.bloomEnabled = v ApplyBloom() end)
    vfx:Slider("Bloom Intensity", 0, 30, 15, function(v) S.bloomIntensity = v / 10 ApplyBloom() end)
    vfx:Slider("Bloom Size", 0, 100, 40, function(v) S.bloomSize = v ApplyBloom() end)
    vfx:Toggle("Color Correction", false, function(v) S.colorEnabled = v ApplyColorCorrection() end)
    vfx:Slider("Brightness", -10, 10, 0, function(v) S.ccBrightness = v / 10 ApplyColorCorrection() end)
    vfx:Slider("Contrast", 0, 20, 1, function(v) S.ccContrast = v / 10 ApplyColorCorrection() end)
    vfx:Slider("Saturation", 0, 20, 2, function(v) S.ccSaturation = v / 10 ApplyColorCorrection() end)
    vfx:Toggle("Sun Rays", false, function(v) S.sunRaysEnabled = v ApplySunRays() end)
    vfx:Toggle("Fog", false, function(v) S.fogEnabled = v ApplyFog() end)
    vfx:Slider("Fog End", 100, 2000, 500, function(v) S.fogEnd = v ApplyFog() end)
    
    -- CUSTOM MODEL
    local mdl = Tab:NewGroupbox("Right", "Custom Model")
    mdl:Dropdown("Select", ModelList, "None", function(v) S.selectedModel = v end)
    mdl:Button("Apply Model", function() if S.selectedModel ~= "None" then ApplyModel(S.selectedModel) end end)
    mdl:Button("Remove Model", function() if getgenv().modelConn then getgenv().modelConn:Disconnect() end S.selectedModel = "None" end)
    
    -- CUSTOM SKYBOX
    local sky = Tab:NewGroupbox("Right", "Custom Skybox")
    sky:Dropdown("Select", SkyboxList, "None", function(v) S.selectedSkybox = v end)
    sky:Toggle("Stars Effect", true, function(v) S.skyboxStarsEnabled = v end)
    sky:Slider("Stars Count", 5, 50, 20, function(v) S.skyboxStarsCount = v end)
    sky:Button("Apply Skybox", function() if S.selectedSkybox and S.selectedSkybox ~= "None" then ApplySkybox(S.selectedSkybox) end end)
    sky:Button("Remove Skybox", function() RemoveSkybox() end)
end

-- EXPLOITS (BETA FEATURES)
do
    local Tab = Win:NewTab("Exploits")
    
    -- EXPLOIT POSITION
    local ep = Tab:NewGroupbox("Left", "Exploit Position")
    ep:Toggle("Enable", false, function(v) 
        S.epEnabled = v 
        if not v and R.epActive then
            EP_Free()
        end
        ApplySettings()
    end)
    ep:Keybind("Key", Enum.KeyCode.C, function(k) S.epKey = k end)
    ep:Slider("Distance", 1, 10, S.epDist, function(v) S.epDist = v end)
    
    -- INFINITY JUMP
    local ij = Tab:NewGroupbox("Left", "Infinity Jump")
    ij:Toggle("Enable", false, function(v) 
        S.ijEnabled = v 
        if not v then
            IJ_RemovePart()
        end
        ApplySettings()
    end)
    ij:Keybind("Key", Enum.KeyCode.Space, function(k) S.ijKey = k end)
    
    -- BARREL EXTEND
    local be = Tab:NewGroupbox("Right", "Barrel Extend")
    be:Toggle("Enable", false, function(v) 
        S.beEnabled = v 
        if not v and R.beActive then
            BE_Disable()
        end
        ApplySettings()
    end)
    be:Keybind("Key", Enum.KeyCode.G, function(k) S.beKey = k end)
    be:Dropdown("Mode", {"Hold","Toggle"}, "Hold", function(v) S.beMode = v end)
    be:Slider("Distance", 1, 15, S.beDist, function(v) S.beDist = v end)
end

-- MISC
do
    local Tab = Win:NewTab("Misc")
    
    local bh = Tab:NewGroupbox("Left", "BunnyHop")
    bh:Toggle("Enable", false, function(v) 
        S.bhEnabled = v 
        if v then 
            if R.myHum then R.bhOrigSpeed = R.myHum.WalkSpeed end
            R.bhInAir, R.bhLastReset, R.bhResetting = false, 0, false
            R.bhLastPos, R.bhPosCheckTime, R.bhCircling = nil, 0, false
            if R.myHum then R.myHum.WalkSpeed = S.bhGroundSpeed end
        else
            if R.myHum then R.myHum.WalkSpeed = R.bhOrigSpeed or 16 end
            R.bhInAir, R.bhResetting = false, false
        end
        ApplySettings() 
    end)
    bh:Keybind("Key", Enum.KeyCode.F, function(k) S.bhKey = k end)
    bh:Slider("Ground Speed", 20, 50, 35, function(v) 
        S.bhGroundSpeed = v 
        if S.bhEnabled and R.myHum and not R.bhInAir and not R.bhResetting then
            R.myHum.WalkSpeed = v
        end
    end)
    bh:Slider("Air Speed", 20, 50, 39, function(v) 
        S.bhAirSpeed = v 
        if S.bhEnabled and R.myHum and R.bhInAir and not R.bhResetting then
            R.myHum.WalkSpeed = v
        end
    end)
    
    local wp = Tab:NewGroupbox("Right", "Wallbang Helper")
    wp:Toggle("Enable", false, function(v)
        S.wbEnabled = v
        if v then
            enableWallbangHelper()
            R.hotkeys["Wallbang"] = {active = true, key = "ON"}
        else
            disableWallbangHelper()
            R.hotkeys["Wallbang"] = nil
        end
        UpdateHotkeyList()
    end)
    
    -- Wallbang Map
    local wm = Tab:NewGroupbox("Right", "Wallbang Map")
    S.wmEnabled = false
    
    wm:Toggle("Enable", false, function(v)
        S.wmEnabled = v
        if v then
            enableWallbangMap()
            R.hotkeys["WB Map"] = {active = true, key = "ON"}
        else
            disableWallbangMap()
            R.hotkeys["WB Map"] = nil
        end
        UpdateHotkeyList()
    end)
    
    local rc = Tab:NewGroupbox("Right", "Remove Collision")
    rc:Toggle("Enable", false, function(v)
        S.rcEnabled = v
        if v then
            enableRemoveCollision()
            R.hotkeys["NoCollision"] = {active = true, key = "ON"}
        else
            disableRemoveCollision()
            R.hotkeys["NoCollision"] = nil
        end
        UpdateHotkeyList()
    end)
    
    -- Teleport
    local tp = Tab:NewGroupbox("Left", "Teleport")
    
    local function teleportToCT()
        if not S.tpEnabled or not R.myHRP then return end
        pcall(function()
            local ctSpawn = WS:FindFirstChild("CTSpawn")
            if ctSpawn and ctSpawn:GetChildren()[4] then
                local target = ctSpawn:GetChildren()[4]
                R.myHRP.CFrame = (target:IsA("BasePart") and target.CFrame or target.PrimaryPart.CFrame) + Vector3.new(0, 3, 0)
            end
        end)
    end
    
    local function teleportToT()
        if not S.tpEnabled or not R.myHRP then return end
        pcall(function()
            local tSpawn = WS:FindFirstChild("TSpawn")
            if tSpawn and tSpawn:GetChildren()[5] then
                local target = tSpawn:GetChildren()[5]
                R.myHRP.CFrame = (target:IsA("BasePart") and target.CFrame or target.PrimaryPart.CFrame) + Vector3.new(0, 3, 0)
            end
        end)
    end
    
    getgenv().teleportToCT = teleportToCT
    getgenv().teleportToT = teleportToT
    
    tp:Toggle("Enable", false, function(v)
        S.tpEnabled = v
        R.hotkeys["Teleport"] = v and {active = true, key = "ON"} or nil
        UpdateHotkeyList()
    end)
    tp:Keybind("CT Spawn", Enum.KeyCode.One, function(k) S.tpCTKey = k end)
    tp:Keybind("T Spawn", Enum.KeyCode.Two, function(k) S.tpTKey = k end)
    
    -- DEBUG CONSOLE
    local dc = Tab:NewGroupbox("Left", "Debug Console")
    dc:Toggle("Enable", false, function(v) 
        S.dcEnabled = v 
        if v and not R.dcFrame then
            DC_CreateUI()
        end
    end)
    dc:Toggle("Notify", false, function(v) S.dcNotify = v end)
    dc:Keybind("Toggle Key", Enum.KeyCode.P, function(k) S.dcKey = k end)
    dc:Button("Show/Hide", function() DC_Toggle() end)
    dc:Button("Clear", function()
        if R.dcFrame then
            local scroll = R.dcFrame:FindFirstChild("Logs")
            if scroll then
                for _, c in ipairs(scroll:GetChildren()) do
                    if c:IsA("TextLabel") then c:Destroy() end
                end
            end
            R.dcLogs = {}
        end
    end)
    
    -- ANIM BREAKER
    local ab = Tab:NewGroupbox("Right", "Anim Breaker")
    ab:Toggle("Enable", false, function(v) 
        S.abEnabled = v 
        if not v and R.abActive then
            AB_Disable()
        end
    end)
    ab:Keybind("Key", Enum.KeyCode.B, function(k) S.abKey = k end)
    
    -- AUTO DOUBLE TAP
    local adt = Tab:NewGroupbox("Right", "Auto Double Tap")
    adt:Toggle("Enable", false, function(v) S.dtAuto = v end)
    adt:Slider("Delay (ms)", 100, 1000, 200, function(v) S.dtAutoDelay = v end)
end

-- SETTINGS
do
    local Tab = Win:NewTab("Settings")
    local mn = Tab:NewGroupbox("Left", "Menu")
    mn:Keybind("Menu Key", Enum.KeyCode.Delete, function(k) S.menuKey = k end)
    mn:Dropdown("Accent Color", {"Green","Red","Blue","Purple","Cyan","Yellow","Orange","Pink","White"}, "Green", function(v)
        local colors = {
            Green = Color3.fromRGB(168, 247, 50),
            Red = Color3.fromRGB(255, 80, 80),
            Blue = Color3.fromRGB(80, 150, 255),
            Purple = Color3.fromRGB(180, 100, 255),
            Cyan = Color3.fromRGB(80, 255, 255),
            Yellow = Color3.fromRGB(255, 255, 80),
            Orange = Color3.fromRGB(255, 150, 50),
            Pink = Color3.fromRGB(255, 100, 200),
            White = Color3.fromRGB(255, 255, 255)
        }
        local newColor = colors[v] or Color3.fromRGB(168, 247, 50)
        T.Accent = newColor
        S.accentColor = v
        
        -- Invalid code Invalid code Invalid code Invalid code Invalid code
        for _, elem in ipairs(R.accentElements) do
            pcall(function()
                if elem.type == "toggle" and elem.box and elem.box.Parent then
                    if elem.getEnabled and elem.getEnabled() then
                        elem.box.BackgroundColor3 = newColor
                    end
                elseif elem.type == "slider" and elem.fill and elem.fill.Parent then
                    elem.fill.BackgroundColor3 = newColor
                elseif elem.type == "dropdown" and elem.buttons then
                    local cur = elem.getCurrent and elem.getCurrent() or ""
                    for _, btn in ipairs(elem.buttons) do
                        if btn and btn.Parent then
                            btn.TextColor3 = btn.Text == cur and newColor or T.Dim
                        end
                    end
                elseif elem.type == "tab" and elem.btn and elem.btn.Parent then
                    -- Invalid code Invalid code Invalid code
                    if elem.page and elem.page.Visible then
                        elem.btn.TextColor3 = newColor
                    end
                end
            end)
        end
        
        -- Invalid code scrollbars
        if R.gui then
            for _, desc in ipairs(R.gui:GetDescendants()) do
                if desc:IsA("ScrollingFrame") then
                    desc.ScrollBarImageColor3 = newColor
                end
            end
        end
        
        -- Invalid code Invalid code
        local function updateGradients(frame)
            if not frame then return end
            for _, child in ipairs(frame:GetChildren()) do
                if child:IsA("Frame") and child.Size.Y.Offset == 2 then
                    local grad = child:FindFirstChildOfClass("UIGradient")
                    if grad then 
                        grad.Color = ColorSequence.new({
                            ColorSequenceKeypoint.new(0, newColor), 
                            ColorSequenceKeypoint.new(1, Color3.fromRGB(newColor.R * 150, newColor.G * 200, newColor.B * 130))
                        }) 
                    end
                end
            end
        end
        updateGradients(R.hkFrame)
        updateGradients(R.wmFrame)
        updateGradients(R.timeFrame)
    end)
    mn:Button("Unload Script", function()
        R.running = false
        StopMainLoop()
        AP_Disable()
        GP_Disable()
        EP_Free()
        BE_Disable()
        AB_Disable()
        IJ_RemovePart()
        HitAir_Remove()
        if R.dcFrame then pcall(function() R.dcFrame:Destroy() end) R.dcFrame = nil end
        if S.bhEnabled and R.myHum then R.myHum.WalkSpeed = R.bhOrigSpeed end
        if S.wbEnabled then disableWallbangHelper() end
        if S.rcEnabled then disableRemoveCollision() end
        if S.wmEnabled then disableWallbangMap() end
        if R.bloomEffect then R.bloomEffect:Destroy() end
        if R.blurEffect then R.blurEffect:Destroy() end
        if R.colorEffect then R.colorEffect:Destroy() end
        if R.sunRaysEffect then R.sunRaysEffect:Destroy() end
        Light.FogStart = 0
        Light.FogEnd = 100000
        for _, c in ipairs(R.conns) do pcall(function() c:Disconnect() end) end
        for _, e in ipairs(R.fx) do pcall(function() e:Destroy() end) end
        if R.gui then R.gui:Destroy() end
    end)
    
    local cf = Tab:NewGroupbox("Right", "Configs")
    cf:ConfigDropdown("Select Config")
    local cfgInput = ""
    cf:TextBox("New Config Name", "", function(t) cfgInput = t end)
    cf:Button("Create & Save", function() 
        if cfgInput ~= "" then 
            SaveCfg(cfgInput) 
            Cfg.Selected = cfgInput 
            if R.cfgDropdown then R.cfgDropdown.Text = cfgInput.." ▼" end 
            UpdateCfgDropdown() 
        end 
    end)
    cf:Button("Save Current", function() 
        if Cfg.Selected ~= "" then 
            SaveCfg(Cfg.Selected) 
        end 
    end)
    cf:Button("Load Selected", function() 
        if Cfg.Selected ~= "" then 
            LoadCfg(Cfg.Selected) 
        end 
    end)
    cf:Button("Delete Selected", function() 
        if Cfg.Selected ~= "" then 
            DeleteCfg(Cfg.Selected) 
            Cfg.Selected = "" 
            if R.cfgDropdown then R.cfgDropdown.Text = "Select... ▼" end 
            UpdateCfgDropdown() 
        end 
    end)
    cf:Button("Refresh", function() UpdateCfgDropdown() end)
end


-- ========== INPUT HANDLER ==========

table.insert(R.conns, UIS.InputBegan:Connect(function(i, gpe)
    if gpe then return end
    
    -- Double Tap (Invalid code Defensive Invalid code)
    if S.dtEnabled and i.KeyCode == S.dtKey then
        DT_Peek(nil)
    end
    
    -- DEBUG CONSOLE TOGGLE
    if i.KeyCode == S.dcKey then
        DC_Toggle()
    end
    
    -- ANIM BREAKER
    if S.abEnabled and i.KeyCode == S.abKey then
        if R.abActive then
            AB_Disable()
        else
            AB_Enable()
        end
    end
    
    -- AI PEEK V4 INPUT
    if S.apEnabled and i.KeyCode == S.apKey then
        if S.apMode == "Hold" then
            if not R.apActive then
                AP_Enable()
            end
        else -- Toggle mode
            if R.apActive then
                AP_Disable()
            else
                AP_Enable()
            end
        end
    end
    
    -- GHOST PEEK INPUT
    if S.gpEnabled and i.KeyCode == S.gpKey then
        if S.gpMode == "Hold" then
            if not R.gpActive then
                GP_Enable()
            end
        else -- Toggle mode
            if R.gpActive then
                GP_Disable()
            else
                GP_Enable()
            end
        end
    end
    
    -- EXPLOIT POSITION
    if S.epEnabled and i.KeyCode == S.epKey then
        if R.epActive then
            EP_DestroyDecoy()
        else
            EP_SpawnDecoy()
        end
    end
    
    -- INFINITY JUMP
    if S.ijEnabled and i.KeyCode == S.ijKey then
        IJ_Jump()
    end
    
    -- BARREL EXTEND
    if S.beEnabled and i.KeyCode == S.beKey then
        if S.beMode == "Hold" then
            if not R.beActive then
                BE_Enable()
            end
        else -- Toggle mode
            if R.beActive then
                BE_Disable()
            else
                BE_Enable()
            end
        end
    end
    
    -- Fakeduck
    if i.KeyCode == S.fdKey then ToggleFakeduck() end
    if i.KeyCode == S.fdLockKey then R.fdLock = true end
    
    -- BunnyHop toggle
    if i.KeyCode == S.bhKey then
        S.bhEnabled = not S.bhEnabled
        if S.bhEnabled then
            R.bhOrigSpeed = R.myHum and R.myHum.WalkSpeed or 16
            R.bhInAir, R.bhLastReset, R.bhResetting = false, 0, false
            R.bhLastPos, R.bhPosCheckTime, R.bhCircling = nil, 0, false
            if R.myHum then R.myHum.WalkSpeed = S.bhGroundSpeed end
        else
            if R.myHum then R.myHum.WalkSpeed = R.bhOrigSpeed end
        end
        ApplySettings()
    end
    
    -- Teleport
    if S.tpEnabled then
        if i.KeyCode == S.tpCTKey then getgenv().teleportToCT() end
        if i.KeyCode == S.tpTKey then getgenv().teleportToT() end
    end
    
    -- Menu toggle
    if i.KeyCode == S.menuKey then
        R.visible = not R.visible
        R.main.Visible = R.visible
        for _, e in ipairs(R.fx) do e:Destroy() end
        R.fx = {}
        if R.visible then
            local blur = Instance.new("DepthOfFieldEffect")
            blur.FarIntensity, blur.FocusDistance, blur.InFocusRadius, blur.NearIntensity = 0.6, 50, 50, 0.6
            blur.Parent = Light
            R.fx[1] = blur
        end
    end
end))

table.insert(R.conns, UIS.InputEnded:Connect(function(i, gpe)
    if gpe then return end
    
    -- AI PEEK V4 - Invalid code Invalid code (Hold mode)
    if S.apEnabled and i.KeyCode == S.apKey and S.apMode == "Hold" then
        AP_Disable()
    end
    
    -- GHOST PEEK - Invalid code Invalid code (Hold mode)
    if S.gpEnabled and i.KeyCode == S.gpKey and S.gpMode == "Hold" then
        GP_Disable()
    end
    
    -- BARREL EXTEND - Invalid code Invalid code (Hold mode)
    if S.beEnabled and i.KeyCode == S.beKey and S.beMode == "Hold" then
        BE_Disable()
    end
    
    -- Fakeduck lock release
    if i.KeyCode == S.fdLockKey then R.fdLock = false end
end))

-- Character respawn handler
LP.CharacterAdded:Connect(function()
    R.myChar, R.myHRP, R.myHead, R.myHum = nil, nil, nil, nil
    R.fireShot, R.playerData = nil, {}
    R.playerDataTime, R.fireShotTime = 0, 0
    R.bhInAir, R.bhLastReset, R.bhResetting = false, 0, false
    R.bhLastPos, R.bhPosCheckTime, R.bhCircling = nil, 0, false
    
    -- Invalid code AI Peek V4 Invalid code Invalid code
    if R.apActive then
        AP_Disable()
    end
    R.apTeleporting = false
    R.apOriginalCFrame = nil
    R.apSavedOrigin = nil
    
    -- Invalid code Ghost Peek Invalid code Invalid code
    if R.gpActive then
        GP_Disable()
    end
    R.gpInPeek = false
    
    -- Invalid code Exploit Position Invalid code Invalid code
    EP_Free()
    
    -- Invalid code Barrel Extend Invalid code Invalid code
    if R.beActive then
        BE_Disable()
    end
    
    -- Invalid code Infinity Jump Invalid code Invalid code
    IJ_RemovePart()
    R.ijCanJump = true
    R.ijWasGrounded = true
    
    -- Invalid code Hit Air Invalid code Invalid code
    HitAir_Remove()
    
    -- Invalid code Anim Breaker Invalid code Invalid code
    AB_Disable()
    
    task.wait(0.5)
    CacheChar()
    
    if S.bhEnabled then 
        R.bhOrigSpeed = R.myHum and R.myHum.WalkSpeed or 16 
        if R.myHum then R.myHum.WalkSpeed = S.bhGroundSpeed end
    end
    if S.fdEnabled then
        S.fdEnabled = false
        R.fdIdleAnim, R.fdWalkAnim = nil, nil
        task.wait(0.5)
        ToggleFakeduck()
    end
    if S.wbEnabled then
        task.wait(1)
        enableWallbangHelper()
    end
    if S.rcEnabled then
        task.wait(1)
        enableRemoveCollision()
    end
    if S.wmEnabled then
        task.wait(1)
        enableWallbangMap()
    end
end)

-- ========== INIT ==========
-- Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
task.defer(function()
    CacheChar()
    EnsureCfgFolder()
    RefreshCfg()
    if #Cfg.List > 0 then
        Cfg.Selected = Cfg.List[1]
        if R.cfgDropdown then R.cfgDropdown.Text = Cfg.Selected.." ▼" end
    end
    -- Invalid code Invalid code Invalid code
    task.spawn(SetupKillTracking)
    -- Invalid code Invalid code Invalid code Invalid code MainLoop
    ApplySettings()
end)

local blur = Instance.new("DepthOfFieldEffect")
blur.FarIntensity, blur.FocusDistance, blur.InFocusRadius, blur.NearIntensity = 0.6, 50, 50, 0.6
blur.Parent = Light
R.fx[1] = blur

print("[Arcanum] Loaded! Press DELETE to open menu")

-- ========== RESOLVER LOGIC ==========
-- Resolver Safe: Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code Invalid code
--                Invalid code HP Invalid code <= rbBodyAimHP, Invalid code Invalid code Invalid code Invalid code (bodyaim)
--                Invalid code Invalid code Invalid code Invalid code (DownTorso)
-- Resolver Aggressive: Invalid code Invalid code, Invalid code Invalid code
--                      Invalid code Invalid code Invalid code Invalid code (DownTorso)
-- FireRate = 0ms (Invalid code Invalid code)

getgenv().ArcanumResolver = {
    getTargetHitbox = function(targetChar, targetHP)
        if not S.rbResolver then return S.rbHitbox end
        
        if S.rbResolverMode == "Safe" then
            -- Safe Invalid code: Invalid code Invalid code Invalid code
            local head = targetChar:FindFirstChild("Head")
            local torso = targetChar:FindFirstChild("UpperTorso") or targetChar:FindFirstChild("Torso")
            
            if not head or not torso then return S.rbHitbox end
            
            -- Invalid code HP <= rbBodyAimHP, Invalid code Invalid code Invalid code Invalid code
            if targetHP and targetHP <= S.rbBodyAimHP then
                return "Torso"
            end
            
            -- Invalid code Invalid code Invalid code Invalid code Invalid code
            if R.myHRP then
                local myTorso = R.myChar and (R.myChar:FindFirstChild("UpperTorso") or R.myChar:FindFirstChild("Torso"))
                if myTorso then
                    RayP.FilterDescendantsInstances = {R.myChar}
                    local rayResult = WS:Raycast(myTorso.Position, (head.Position - myTorso.Position), RayP)
                    if rayResult and rayResult.Instance:IsDescendantOf(targetChar) then
                        return "Head" -- Invalid code Invalid code
                    else
                        return "Torso" -- Invalid code Invalid code Invalid code, Invalid code Invalid code Invalid code
                    end
                end
            end
            return "Head"
        else
            -- Aggressive Invalid code: Invalid code Invalid code
            return S.rbHitbox
        end
    end,
    
    getFireOrigin = function()
        -- Invalid code Invalid code Invalid code Invalid code (DownTorso)
        if R.myChar then
            local torso = R.myChar:FindFirstChild("LowerTorso") or R.myChar:FindFirstChild("UpperTorso") or R.myChar:FindFirstChild("Torso")
            if torso then
                return torso.Position
            end
        end
        return R.myHRP and R.myHRP.Position or V3_ZERO
    end,
    
    shouldFire = function(targetChar, targetHP)
        if not S.rbEnabled then return false end
        
        if S.rbResolverMode == "Safe" then
            -- Safe: Invalid code Invalid code Invalid code Invalid code Invalid code
            local hitbox = getgenv().ArcanumResolver.getTargetHitbox(targetChar, targetHP)
            local targetPart = targetChar:FindFirstChild(hitbox == "Head" and "Head" or "UpperTorso") or targetChar:FindFirstChild("Torso")
            if not targetPart then return false end
            
            local origin = getgenv().ArcanumResolver.getFireOrigin()
            RayP.FilterDescendantsInstances = {R.myChar}
            local rayResult = WS:Raycast(origin, (targetPart.Position - origin), RayP)
            return rayResult and rayResult.Instance:IsDescendantOf(targetChar)
        else
            -- Aggressive: Invalid code Invalid code
            return true
        end
    end
}
