

local function addHighlightToMesh(mesh)
    if not mesh:FindFirstChild(highlightName) then
        local highlight = Instance.new("Highlight")
        highlight.Name = highlightName
        highlight.Parent = mesh
        highlight.FillColor = Color3.fromRGB(0, 255, 0)
        highlight.OutlineColor = Color3.fromRGB(255, 255, 0)
        highlight.FillTransparency = 0.3
        highlight.OutlineTransparency = 0
    end
end

local function removeHighlightFromMesh(mesh)
    local highlight = mesh:FindFirstChild(highlightName)
    if highlight then
        highlight:Destroy()
    end
end
})

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "JP Hub",
   Icon = 0, -- Icon in Topbar. Can use Lucide Icons (string) or Roblox Image (number). 0 to use no icon (default).
   LoadingTitle = "JP Hub",
   LoadingSubtitle = "by Sirius",
   ShowText = "JP Hub", -- for mobile users to unhide rayfield, change if you'd like
   Theme = "Dark", -- Check https://docs.sirius.menu/rayfield/configuration/themes

   ToggleUIKeybind = "K", -- The keybind to toggle the UI visibility (string like "K" or Enum.KeyCode)

   DisableRayfieldPrompts = false,
   DisableBuildWarnings = false, -- Prevents Rayfield from warning when the script has a version mismatch with the interface

   ConfigurationSaving = {
      Enabled = true,
      FolderName = nil, -- Create a custom folder for your hub/game
      FileName = "Big Hub"
   },

   Discord = {
      Enabled = false, -- Prompt the user to join your Discord server if their executor supports it
      Invite = "noinvitelink", -- The Discord invite code, do not include discord.gg/. E.g. discord.gg/ ABCD would be ABCD
      RememberJoins = true -- Set this to false to make them join the discord every time they load it up
   },

   KeySystem = false, -- Set this to true to use our key system
   KeySettings = {
      Title = "Untitled",
      Subtitle = "Key System",
      Note = "No method of obtaining the key is provided", -- Use this to tell the user how to get a key
      FileName = "Key", -- It is recommended to use something unique as other scripts using Rayfield may overwrite your key file
      SaveKey = true, -- The user's key will be saved, but if you change the key, they will be unable to use your script
      GrabKeyFromSite = false, -- If this is true, set Key below to the RAW site you would like Rayfield to get the key from
      Key = {"Hello"} -- List of keys that will be accepted by the system, can be RAW file links (pastebin, github etc) or simple strings ("hello","key22")
   }
})

local Maintab = Window:CreateTab("Maintab", 4483362458) -- Title, Image

local Toggle = Maintab:CreateToggle({
    Name = "Esp",
    CurrentValue = false,
    Flag = "Esp",
    Callback = function(Value)
        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer then
                local character = player.Character
                if character then
                    -- Ativar ESP
                    if Value then
                        -- Adiciona destaque vermelho
                        if not character:FindFirstChild("EspHighlight") then
                            local highlight = Instance.new("Highlight")
                            highlight.Name = "EspHighlight"
                            highlight.FillColor = Color3.fromRGB(255, 0, 0)
                            highlight.OutlineColor = Color3.fromRGB(255, 150, 150)
                            highlight.FillTransparency = 0.6
                            highlight.OutlineTransparency = 0
                            highlight.Adornee = character
                            highlight.Parent = character
                        end

                        -- Adiciona o nome pequeno acima do jogador
                        if not character:FindFirstChild("EspName") then
                            local billboard = Instance.new("BillboardGui")
                            billboard.Name = "EspName"
                            billboard.Size = UDim2.new(0, 100, 0, 30)
                            billboard.Adornee = character:FindFirstChild("Head")
                            billboard.AlwaysOnTop = true
                            billboard.StudsOffset = Vector3.new(0, 2.5, 0)

                            local nameLabel = Instance.new("TextLabel")
                            nameLabel.Parent = billboard
                            nameLabel.BackgroundTransparency = 1
                            nameLabel.Text = player.Name
                            nameLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
                            nameLabel.TextScaled = false
                            nameLabel.TextSize = 10
                            nameLabel.Font = Enum.Font.SourceSansBold
                            nameLabel.Size = UDim2.new(1, 0, 1, 0)

                            billboard.Parent = character
                        end
                    else
                        -- Desativa ESP (remove tudo)
                        local highlight = character:FindFirstChild("EspHighlight")
                        if highlight then highlight:Destroy() end

                        local billboard = character:FindFirstChild("EspName")
                        if billboard then billboard:Destroy() end
                    end
                end
            end
        end
    end,
})

-- Atualiza automaticamente quando novos jogadores entram
game.Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function(character)
        if Toggle.CurrentValue then
            local highlight = Instance.new("Highlight")
            highlight.Name = "EspHighlight"
            highlight.FillColor = Color3.fromRGB(255, 0, 0)
            highlight.OutlineColor = Color3.fromRGB(255, 150, 150)
            highlight.FillTransparency = 0.6
            highlight.OutlineTransparency = 0
            highlight.Adornee = character
            highlight.Parent = character

            local billboard = Instance.new("BillboardGui")
            billboard.Name = "EspName"
            billboard.Size = UDim2.new(0, 100, 0, 30)
            billboard.Adornee = character:WaitForChild("Head")
            billboard.AlwaysOnTop = true
            billboard.StudsOffset = Vector3.new(0, 2.5, 0)

            local nameLabel = Instance.new("TextLabel")
            nameLabel.Parent = billboard
            nameLabel.BackgroundTransparency = 1
            nameLabel.Text = player.Name
            nameLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
            nameLabel.TextScaled = false
            nameLabel.TextSize = 10
            nameLabel.Font = Enum.Font.SourceSansBold
            nameLabel.Size = UDim2.new(1, 0, 1, 0)

            billboard.Parent = character
        end
    end,
end
})

local highlightName = "RaelHubESPHighlight"

local meshesFolder = game.Workspace["a046909c-1905-f6be-b42f-ab76084d0000"].Map.Tasks.Computer.Meshes

local Toggle = Maintab:CreateToggle({
   Name = "Esp",
   CurrentValue = false,
   Flag = "Esp",
   Callback = function(Value)
      for _, mesh in ipairs(meshesFolder:GetChildren()) do
        -- Garante que só adiciona Highlight em objetos "MeshPart" (ou altere para outro tipo se necessário)
        if mesh:IsA("BasePart") then
            if Value then
                addHighlightToMesh(mesh)
            else
                removeHighlightFromMesh(mesh)
            end
        end
      end
   end,
}) 
