local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "Suc0HUB",
   Icon = 0, -- Icon in Topbar. Can use Lucide Icons (string) or Roblox Image (number). 0 to use no icon (default).
   LoadingTitle = "Suc0HUB",
   LoadingSubtitle = "by suc0",
   Theme = "Default", -- Check https://docs.sirius.menu/rayfield/configuration/themes

   DisableRayfieldPrompts = false,
   DisableBuildWarnings = false, 
   ConfigurationSaving = {
      Enabled = true,
      FolderName = nil, 
      FileName = "Suc0HUB"
   },

   Discord = {
      Enabled = false, 
      Invite = "", 
      RememberJoins = true 
   },

   KeySystem = true, 
   KeySettings = {
      Title = "Sistema de key",
      Subtitle = "suc0HUB",
      Note = "peça a key pra mim bb", -- Use this to tell the user how to get a key
      FileName = "Key", 
      SaveKey = true, 
      GrabKeyFromSite = false, -- If this is true, set Key below to the RAW site you would like Rayfield to get the key from
      Key = {"SenhaFodasticaSUCAO"}
   }
})

local Tab = Window:CreateTab("Beaks", 13573694944) 

local Section = Tab:CreateSection("Beaks")

Section:Set("Beaks farm")

local Toggle = Tab:CreateToggle({
   Name = "esp passaros a 60 metros de distância",
   CurrentValue = false,
   Flag = "Toggle1", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
   local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local Player = game.Players.LocalPlayer

local espObjects = {}
local maxDistance = 60 -- metros

function createText()
    local text = Drawing.new("Text")
    text.Size = 16
    text.Center = true
    text.Outline = true
    text.Color = Color3.fromRGB(255, 255, 255)
    text.Visible = false
    return text
end

function addBeakESP(model)
    if not espObjects[model] and model:IsA("Model") and model.Name == "Beak" then
        local basePart = model.PrimaryPart or model:FindFirstChildWhichIsA("BasePart")
        if basePart then
            local text = createText()
            espObjects[model] = text

            model.AncestryChanged:Connect(function(_, parent)
                if not parent then
                    text:Remove()
                    espObjects[model] = nil
                end
            end)
        end
    end
end

for _, obj in pairs(workspace:GetDescendants()) do
    addBeakESP(obj)
end

workspace.DescendantAdded:Connect(function(obj)
    task.wait(0.1)
    addBeakESP(obj)
end)

RunService.RenderStepped:Connect(function()
    for model, text in pairs(espObjects) do
        local part = model.PrimaryPart or model:FindFirstChildWhichIsA("BasePart")
        if part and Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
            local distance = (part.Position - Player.Character.HumanoidRootPart.Position).Magnitude
            if distance <= maxDistance then
                local pos, onScreen = Camera:WorldToViewportPoint(part.Position)
                if onScreen then
                    text.Position = Vector2.new(pos.X, pos.Y)
                    text.Text = "Pássaro (" .. math.floor(distance) .. "m)"
                    text.Visible = true
                else
                    text.Visible = false
                end
            else
                text.Visible = false
            end
        else
            text.Visible = false
        end
    end
end)
   
   
   end,
})

Toggle:Set(false)


