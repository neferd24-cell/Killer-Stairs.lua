-- ⚡ Panel del Jugador (versión con velocidad permanente)
local player = game.Players.LocalPlayer

-- Esperar a que el personaje cargue siempre que reaparezca
local function setupChar(char)
	local humanoid = char:WaitForChild("Humanoid")
	humanoid.WalkSpeed = 40 -- ⚡ Velocidad inicial más rápida
end

player.CharacterAdded:Connect(setupChar)
if player.Character then
	setupChar(player.Character)
end

-- Crear la interfaz
local gui = Instance.new("ScreenGui")
gui.Parent = player:WaitForChild("PlayerGui")
gui.ResetOnSpawn = false

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 230, 0, 160)
frame.Position = UDim2.new(0.4, 0, 0.4, 0)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.Active = true
frame.Draggable = true
frame.Parent = gui

local title = Instance.new("TextLabel")
title.Text = "🎮 Panel del Jugador"
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.GothamBold
title.TextScaled = true
title.Parent = frame

-- Botón Subir/Bajar
local btnSubir = Instance.new("TextButton")
btnSubir.Size = UDim2.new(1, -20, 0, 40)
btnSubir.Position = UDim2.new(0, 10, 0, 40)
btnSubir.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
btnSubir.TextColor3 = Color3.fromRGB(255, 255, 255)
btnSubir.Font = Enum.Font.GothamBold
btnSubir.TextScaled = true
btnSubir.Text = "⬆️ Subir / Bajar"
btnSubir.Parent = frame

-- Botón Velocidad
local btnVel = Instance.new("TextButton")
btnVel.Size = UDim2.new(1, -20, 0, 40)
btnVel.Position = UDim2.new(0, 10, 0, 90)
btnVel.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
btnVel.TextColor3 = Color3.fromRGB(255, 255, 255)
btnVel.Font = Enum.Font.GothamBold
btnVel.TextScaled = true
btnVel.Text = "⚡ Aumentar Velocidad"
btnVel.Parent = frame

-- Botón cerrar
local btnCerrar = Instance.new("TextButton")
btnCerrar.Size = UDim2.new(1, -20, 0, 25)
btnCerrar.Position = UDim2.new(0, 10, 1, -30)
btnCerrar.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
btnCerrar.TextColor3 = Color3.fromRGB(255, 255, 255)
btnCerrar.Font = Enum.Font.GothamBold
btnCerrar.TextScaled = true
btnCerrar.Text = "❌ Cerrar"
btnCerrar.Parent = frame

-- Crear piso invisible (alto y enorme)
local piso = Instance.new("Part")
piso.Size = Vector3.new(10000, 2, 10000)
piso.Position = Vector3.new(0, 1500, 0)
piso.Anchored = true
piso.CanCollide = true
piso.Transparency = 1
piso.Name = "PisoInvisible"
piso.Parent = workspace

-- Variables
local arriba = false
local velocidad = 40

-- Botón subir/bajar
btnSubir.MouseButton1Click:Connect(function()
	local char = player.Character
	if not char then return end
	local hrp = char:FindFirstChild("HumanoidRootPart")
	if not hrp then return end

	if not arriba then
		hrp.CFrame = CFrame.new(piso.Position + Vector3.new(0, 5, 0))
		arriba = true
	else
		hrp.CFrame = hrp.CFrame - Vector3.new(0, 1500, 0)
		arriba = false
	end
end)

-- Botón velocidad
btnVel.MouseButton1Click:Connect(function()
	local char = player.Character
	if not char then return end
	local humanoid = char:FindFirstChild("Humanoid")
	if not humanoid then return end

	velocidad = velocidad + 20
	if velocidad > 120 then velocidad = 40 end
	humanoid.WalkSpeed = velocidad
	btnVel.Text = "⚡ Velocidad: " .. velocidad
end)

-- Botón cerrar
btnCerrar.MouseButton1Click:Connect(function()
	gui:Destroy()
end
