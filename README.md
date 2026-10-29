
-- DUELS GUI V2 | START SCREEN + KEY
-- Para tu propia experiencia en Roblox Studio

local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local KEY_CORRECTA = "DUELS V2"

local blue = Color3.fromRGB(0, 130, 255)
local dark = Color3.fromRGB(12, 15, 25)
local white = Color3.fromRGB(245, 248, 255)
local red = Color3.fromRGB(255, 55, 65)
local green = Color3.fromRGB(0, 160, 100)

local gui = Instance.new("ScreenGui")
gui.Name = "DuelsGUI_V2"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.Parent = player:WaitForChild("PlayerGui")

local function round(obj, radius)
    local c = Instance.new("UICorner")
    c.CornerRadius = UDim.new(0, radius or 10)
    c.Parent = obj
end

local function outline(obj, color)
    local s = Instance.new("UIStroke")
    s.Color = color
    s.Thickness = 2
    s.Parent = obj
    return s
end

-- PANTALLA DE INICIO / KEY
local start = Instance.new("Frame")
start.Size = UDim2.fromOffset(330, 245)
start.Position = UDim2.new(0.5, -165, 0.5, -122)
start.BackgroundColor3 = dark
start.Parent = gui
round(start, 15)
outline(start, blue)

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -20, 0, 45)
title.Position = UDim2.fromOffset(10, 10)
title.BackgroundTransparency = 1
title.Text = "DUELS GUI V2"
title.TextColor3 = blue
title.Font = Enum.Font.GothamBold
title.TextSize = 25
title.Parent = start

local subtitle = Instance.new("TextLabel")
subtitle.Size = UDim2.new(1, -20, 0, 25)
subtitle.Position = UDim2.fromOffset(10, 53)
subtitle.BackgroundTransparency = 1
subtitle.Text = "BIENVENIDO | INTRODUCE TU KEY"
subtitle.TextColor3 = white
subtitle.Font = Enum.Font.Gotham
subtitle.TextSize = 12
subtitle.Parent = start

local keyBox = Instance.new("TextBox")
keyBox.Size = UDim2.new(1, -40, 0, 42)
keyBox.Position = UDim2.fromOffset(20, 91)
keyBox.BackgroundColor3 = Color3.fromRGB(25, 31, 47)
keyBox.TextColor3 = white
keyBox.PlaceholderText = "Escribe la KEY..."
keyBox.PlaceholderColor3 = Color3.fromRGB(140, 150, 170)
keyBox.Text = ""
keyBox.TextSize = 16
keyBox.Font = Enum.Font.GothamBold
keyBox.ClearTextOnFocus = false
keyBox.Parent = start
round(keyBox, 8)

local enter = Instance.new("TextButton")
enter.Size = UDim2.new(1, -40, 0, 40)
enter.Position = UDim2.fromOffset(20, 143)
enter.BackgroundColor3 = blue
enter.TextColor3 = white
enter.Text = "VERIFICAR KEY"
enter.Font = Enum.Font.GothamBold
enter.TextSize = 14
enter.Parent = start
round(enter, 8)

local status = Instance.new("TextLabel")
status.Size = UDim2.new(1, -20, 0, 28)
status.Position = UDim2.fromOffset(10, 190)
status.BackgroundTransparency = 1
status.Text = "KEY requerida para continuar"
status.TextColor3 = Color3.fromRGB(170, 180, 200)
status.Font = Enum.Font.Gotham
status.TextSize = 11
status.Parent = start

-- VARIABLES DEL MENU
local menu
local speedBox
local speedOn = false
local jumpOn = false
local noclipOn = false
local murderOn = false
local sheriffOn = false
local originalSpeed = 16
local originalCollision = {}
local highlights = {}

local function getHumanoid()
    local char = player.Character
    return char and char:FindFirstChildOfClass("Humanoid")
end

local function launchMenu()
    start:Destroy()

    menu = Instance.new("Frame")
    menu.Size = UDim2.fromOffset(440, 310)
    menu.Position = UDim2.new(0.5, -220, 0.5, -155)
    menu.BackgroundColor3 = dark
    menu.Parent = gui
    round(menu, 12)

    local stroke = outline(menu, blue)
    Instance.new("UIDragDetector", menu)

    local header = Instance.new("TextLabel")
    header.Size = UDim2.new(1, -50, 0, 42)
    header.Position = UDim2.fromOffset(12, 0)
    header.BackgroundTransparency = 1
    header.Text = "DUELS GUI | V2"
    header.TextColor3 = blue
    header.TextSize = 19
    header.Font = Enum.Font.GothamBold
    header.TextXAlignment = Enum.TextXAlignment.Left
    header.Parent = menu

    local hide = Instance.new("TextButton")
    hide.Size = UDim2.fromOffset(35, 30)
    hide.Position = UDim2.new(1, -40, 0, 5)
    hide.Text = "—"
    hide.TextColor3 = white
    hide.BackgroundColor3 = Color3.fromRGB(45, 48, 65)
    hide.Parent = menu
    round(hide, 7)

    local opener = Instance.new("TextButton")
    opener.Size = UDim2.fromOffset(120, 40)
    opener.Position = UDim2.fromOffset(15, 100)
    opener.Text = "ABRIR DUELS"
    opener.TextColor3 = white
    opener.BackgroundColor3 = dark
    opener.Visible = false
    opener.Parent = gui
    round(opener, 9)
    outline(opener, blue)

    hide.Activated:Connect(function()
        menu.Visible = false
        opener.Visible = true
    end)

    opener.Activated:Connect(function()
        menu.Visible = true
        opener.Visible = false
    end)

    local sidebar = Instance.new("Frame")
    sidebar.Position = UDim2.fromOffset(10, 48)
    sidebar.Size = UDim2.fromOffset(120, 250)
    sidebar.BackgroundColor3 = Color3.fromRGB(20, 25, 39)
    sidebar.Parent = menu
    round(sidebar, 9)

    local content = Instance.new("Frame")
    content.Position = UDim2.fromOffset(140, 48)
    content.Size = UDim2.new(1, -150, 1, -58)
    content.BackgroundTransparency = 1
    content.Parent = menu

    local pages = {}

    local function makePage(name)
        local page = Instance.new("ScrollingFrame")
        page.Name = name
        page.Size = UDim2.fromScale(1, 1)
        page.BackgroundTransparency = 1
        page.BorderSizePixel = 0
        page.ScrollBarThickness = 3
        page.AutomaticCanvasSize = Enum.AutomaticSize.Y
        page.CanvasSize = UDim2.new()
        page.Visible = false
        page.Parent = content

        local layout = Instance.new("UIListLayout")
        layout.Padding = UDim.new(0, 8)
        layout.SortOrder = Enum.SortOrder.LayoutOrder
        layout.Parent = page

        pages[name] = page
        return page
    end

    local function label(page, text, color)
        local obj = Instance.new("TextLabel")
        obj.Size = UDim2.new(1, -4, 0, 32)
        obj.BackgroundColor3 = Color3.fromRGB(25, 31, 47)
        obj.TextColor3 = color or white
        obj.Text = text
        obj.TextSize = 13
        obj.Font = Enum.Font.GothamMedium
        obj.Parent = page
        round(obj, 6)
        return obj
    end

    local function button(page, text, callback)
        local obj = Instance.new("TextButton")
        obj.Size = UDim2.new(1, -4, 0, 35)
        obj.BackgroundColor3 = Color3.fromRGB(30, 42, 63)
        obj.TextColor3 = white
        obj.Text = text
        obj.TextSize = 13
        obj.Font = Enum.Font.GothamMedium
        obj.Parent = page
        round(obj, 6)

        obj.Activated:Connect(function()
            callback(obj)
        end)
        return obj
    end

    local function input(page, placeholder, initial)
        local box = Instance.new("TextBox")
        box.Size = UDim2.new(1, -4, 0, 35)
        box.BackgroundColor3 = Color3.fromRGB(25, 31, 47)
        box.TextColor3 = white
        box.PlaceholderText = placeholder
        box.Text = tostring(initial or "")
        box.TextSize = 13
        box.Font = Enum.Font.Gotham
        box.ClearTextOnFocus = false
        box.Parent = page
        round(box, 6)
        return box
    end

    local function toggle(page, name, callback)
        local enabled = false
        return button(page, name .. ": OFF", function(obj)
            enabled = not enabled
            obj.Text = name .. (enabled and ": ON" or ": OFF")
            obj.BackgroundColor3 = enabled and green
                or Color3.fromRGB(30, 42, 63)
            callback(enabled)
        end)
    end

    -- PAGINAS
    local home = makePage("Inicio")
    local movement = makePage("Movimiento")
    local roles = makePage("Roles")
    local settings = makePage("Ajustes")

    label(home, "BIENVENIDO A DUELS", blue)
    label(home, "Version del menu: V2")
    label(home, "Panel flotante personalizable")
    label(home, "Imagen: agrega un ImageLabel")
    label(home, "con el ID de tu imagen de Roblox.")

    -- MOVIMIENTO
    speedBox = input(movement, "Velocidad (8-32)", 16)

    local function applySpeed()
        local hum = getHumanoid()
        if not hum then return end
        if speedOn then
            hum.WalkSpeed = math.clamp(
                tonumber(speedBox.Text) or 16, 8, 32
            )
        else
            hum.WalkSpeed = originalSpeed
        end
    end

    toggle(movement, "Speed", function(on)
        local hum = getHumanoid()
        if hum and not speedOn then
            originalSpeed = hum.WalkSpeed
        end
        speedOn = on
        applySpeed()
    end)

    button(movement, "Aplicar velocidad", function()
        applySpeed()
    end)

    speedBox.FocusLost:Connect(function()
        if speedOn then applySpeed() end
    end)

    toggle(movement, "Infinity Jump", function(on)
        jumpOn = on
    end)

    toggle(movement, "Noclip", function(on)
        noclipOn = on
        if not on then
            for part, old in pairs(originalCollision) do
                if part and part.Parent then
                    part.CanCollide = old
                end
            end
            table.clear(originalCollision)
        end
    end)

    -- ROLES DE TU PROPIO JUEGO
    local distanceLabel = label(roles, "Distancia: -- studs")

    toggle(roles, "Murderer", function(on)
        murderOn = on
    end)

    toggle(roles, "Sheriff", function(on)
        sheriffOn = on
    end)

    label(roles, "Murderer: rojo | Sheriff: azul", red)

    -- AJUSTES
    local colors = {
        Color3.fromRGB(0, 130, 255),
        Color3.fromRGB(155, 65, 255),
        Color3.fromRGB(0, 220, 160),
        Color3.fromRGB(255, 75, 100),
        Color3.fromRGB(255, 170, 30)
    }
    local colorIndex = 1

    button(settings, "Cambiar color del panel", function()
        colorIndex = colorIndex % #colors + 1
        blue = colors[colorIndex]
        stroke.Color = blue
        header.TextColor3 = blue
        opener.TextColor3 = blue
    end)

    button(settings, "Cambiar tamaño", function()
        if menu.Size.X.Offset == 440 then
            menu.Size = UDim2.fromOffset(360, 270)
        else
            menu.Size = UDim2.fromOffset(440, 310)
        end
    end)

    -- PESTAÑAS
    local names = {"Inicio", "Movimiento", "Roles", "Ajustes"}

    for i, name in ipairs(names) do
        local tab = Instance.new("TextButton")
        tab.Size = UDim2.new(1, -10, 0, 40)
        tab.Position = UDim2.fromOffset(5, 6 + (i - 1) * 48)
        tab.BackgroundColor3 = Color3.fromRGB(30, 38, 56)
        tab.TextColor3 = white
        tab.Text = name
        tab.TextSize = 13
        tab.Font = Enum.Font.GothamBold
        tab.Parent = sidebar
        round(tab, 7)

        tab.Activated:Connect(function()
            for _, page in pairs(pages) do
                page.Visible = false
            end
            pages[name].Visible = true
        end)
    end

    pages.Inicio.Visible = true

    -- FUNCIONES DE MOVIMIENTO
    UIS.JumpRequest:Connect(function()
        if not jumpOn then return end
        local hum = getHumanoid()
        if hum then
            hum:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end)

    RunService.Stepped:Connect(function()
        if not noclipOn then return end
        local char = player.Character
        if not char then return end

        for _, part in ipairs(char:GetDescendants()) do
            if part:IsA("BasePart") then
                if originalCollision[part] == nil then
                    originalCollision[part] = part.CanCollide
                end
                part.CanCollide = false
            end
        end
    end)

    -- DETECCION DE ROLES CONFIGURADOS POR TU JUEGO
    local function getRole(plr)
        local role = plr:GetAttribute("Role")
        if role then return tostring(role):lower() end
        if plr.Team then return plr.Team.Name:lower() end
        return ""
    end

    RunService.Heartbeat:Connect(function()
        local char = player.Character
        local root = char and char:FindFirstChild("HumanoidRootPart")
        local nearest = math.huge

        for _, plr in ipairs(Players:GetPlayers()) do
            if plr ~= player then
                local role = getRole(plr)
                local isMurder = role == "murderer"
                    or role == "murder"
                local isSheriff = role == "sheriff"
                local target = plr.Character
                local targetRoot = target
                    and target:FindFirstChild("HumanoidRootPart")
                local show = (murderOn and isMurder)
                    or (sheriffOn and isSheriff)

                if target and show then
                    local h = highlights[plr]
                    if not h or h.Parent ~= target then
                        if h then h:Destroy() end
                        h = Instance.new("Highlight")
                        h.Parent = target
                        highlights[plr] = h
                    end
                    h.FillColor = isMurder and red or blue
                    h.OutlineColor = h.FillColor
                    h.Enabled = true
                elseif highlights[plr] then
                    highlights[plr]:Destroy()
                    highlights[plr] = nil
                end

                if root and targetRoot and show then
                    nearest = math.min(nearest,
                        (root.Position - targetRoot.Position).Magnitude)
                end
            end
        end

        if distanceLabel.Parent then
            distanceLabel.Text = nearest < math.huge
                and ("Distancia: " .. math.floor(nearest) .. " studs")
                or "Distancia: -- studs"
        end
    end)

    player.CharacterAdded:Connect(function()
        task.wait(0.5)
        local hum = getHumanoid()
        if hum and speedOn then
            originalSpeed = 16
            applySpeed()
        end
        table.clear(originalCollision)
    end)
end

local verifying = false

local function verifyKey()
    if verifying then return end
    verifying = true

    if keyBox.Text == KEY_CORRECTA then
        status.Text = "KEY CORRECTA - CARGANDO..."
        status.TextColor3 = green
        enter.Text = "ACCESO CONCEDIDO"
        enter.BackgroundColor3 = green
        task.wait(0.5)
        launchMenu()
    else
        status.Text = "KEY INCORRECTA. INTENTA OTRA VEZ."
        status.TextColor3 = red
        keyBox.Text = ""
        verifying = false
    end
end

enter.Activated:Connect(verifyKey)

keyBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        verifyKey()
    end
end)

print("DUELS GUI V2: pantalla de inicio cargada")

