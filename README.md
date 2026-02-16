local player = game.Players.LocalPlayer

local gui = Instance.new("ScreenGui")
gui.Name = "ArgResultGui"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

local box = Instance.new("TextBox")
box.Size = UDim2.fromScale(0.8, 0.5)
box.Position = UDim2.fromScale(0.1, 0.1)
box.BackgroundTransparency = 0.2
box.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
box.TextColor3 = Color3.fromRGB(255, 255, 255)
box.TextWrapped = true
box.TextXAlignment = Enum.TextXAlignment.Left
box.TextYAlignment = Enum.TextYAlignment.Top
box.TextSize = 20
box.TextEditable = false
box.ClearTextOnFocus = false
box.MultiLine = true
box.Parent = gui

local copyButton = Instance.new("TextButton")
copyButton.Size = UDim2.fromScale(0.35, 0.1)
copyButton.Position = UDim2.fromScale(0.325, 0.65)
copyButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
copyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
copyButton.TextSize = 22
copyButton.Text = "COPY"
copyButton.Parent = gui

local folder = workspace:WaitForChild("ArgSigns")
local results = {}

for i = 1, 9 do
    local signModel = folder:FindFirstChild("ARGSign" .. i)
    if signModel then
        for _, obj in ipairs(signModel:GetDescendants()) do
            if obj:IsA("TextLabel") then
                local text = obj.Text:gsub("%d", ""):gsub("%-", "")
                if text ~= "" then
                    table.insert(results, text)
                end
                break
            end
        end
    end
end

local finalText = table.concat(results, ""):gsub("%s+", ""):gsub("^/e", "/e ")
box.Text = finalText

copyButton.MouseButton1Click:Connect(function()
    if setclipboard then
        setclipboard(box.Text)
        copyButton.Text = "COPIED!"
        task.delay(1, function()
            copyButton.Text = "COPY"
        end)
    else
        copyButton.Text = "FAILED"
    end
end)
