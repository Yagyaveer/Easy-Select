# Easy-Select

Main Lua Code:

```lua
B:toh Stage Editor By I_Yagyaveer
Date Created: 22-10-2021
Last Modified: 22-10-2021
]]
-- Variables
local Toolbar = plugin:CreateToolbar("I_Yagyaveer's Plugins") -- Creats The Toolbar Of The Pligin
local ToolbarButton = Toolbar:CreateButton("Easy Select", "Edit Selection With Code", "") -- Creates A Button In The Toolbar
ToolbarButton.ClickableWhenViewportHidden = true -- Makes It Clickable When Viewport Hidden
local Plugin = plugin:CreateDockWidgetPluginGui( -- Create The DockWidgetPluginGui
	"Easy Select", -- Name
	DockWidgetPluginGuiInfo.new( -- Info
		Enum.InitialDockState.Float, -- Sets it so that it will float
		false,
		false,
		521,
		183,
		521,
		183
	)
)
Plugin.Title = "Easy Select" -- Title Of The Plugin
Plugin.Name = "Easy Select" -- Name Of The Plugin (already done but lol)
local MainGui = script.EasySelect:Clone() -- The UI Of The Plugin
MainGui.Parent = Plugin -- sends The UI To The Plugin
local TweenService = game:GetService("TweenService") -- Tween Service
local CurrentSelections = {} -- The Current Selection By The User
local RunService = game:GetService("RunService") -- Run Service
local MainFrame = MainGui.Frame -- A Frame The The UI
local Value = MainFrame.Options.Properties.Value.TextBox  -- Another Frame In The UI
local TextValue = MainFrame.Options.ASelecton.Value.Button
local User = game.Players.LocalPlayer
local TextSize = 14
--Functions
--Game Selection
MainGui.Actions.Apply.MouseButton1Click:Connect(function()
	local Suc,Err = pcall(function()
		print(tostring(Value.Text))
		loadstring(tostring("local Selection = game.Selection:Get()\n"..Value.Text))()
	end)
	if not Suc then
		error(Err)
	end
end)
MainFrame.Options.SizeTab.Label.Button.MouseWheelForward:Connect(function()
	TextSize = math.clamp(TextSize+1,0,100)
end)
MainFrame.Options.SizeTab.Label.Button.MouseWheelBackward:Connect(function()
	TextSize = math.clamp(TextSize-1,0,100)
end)
RunService.RenderStepped:Connect(function()
	local Selcetion = game.Selection:Get()
	CurrentSelections = Selcetion
	MainFrame.Options.SizeTab.Label.Button.Text = "Text Size: " .. tostring(TextSize)
	MainFrame.Options.Properties.Value.TextBox.TextSize = tonumber(TextSize)
	MainFrame.Options.Properties.Value.DisplayText.TextSize = tonumber(TextSize)
	if MainFrame.Options.Properties.Value.TextBox.Text == "" then
		MainFrame.Options.Properties.Value.TextBox.TextTransparency = 0
	else
		MainFrame.Options.Properties.Value.TextBox.TextTransparency = 1
	end
end)
spawn(function()
	while wait() do
		local String = MainFrame.Options.Properties.Value.TextBox.Text
		local Str = string.gsub(String, "%w+",
			{Selection='<font color="#00FF00">Selection</font>',
				[". "] = '<font color="#FFFFFF">.</font>',
				[" = "] = '<font color="#FFFFFF">=</font>',
				["local"] = '<font color="#f86d7c">local</font>',
				["spawn"] = '<font color="#47a0f7">spawn</font>',
				["do"] = '<font color="#f86d7c">do</font>',
				["function"] = '<font color="#f86d7c">function</font>',
				["if"] = '<font color="#f86d7c">if</font>',
				["then"] = '<font color="#f86d7c">then</font>',
				["else"] = '<font color="#f86d7c">else</font>',
				["end"] = '<font color="#f86d7c">end</font>',
				["nil"] = '<font color="#f86d7c">nil</font>',
				["Connect"] = '<font color="#fdbb57">Connect</font>',
				["wait"] = '<font color="#47a0f7">wait</font>',
				["for"] = '<font color="#f86d7c">for</font>',
				["in"] = '<font color="#f86d7c">in</font>',
				["pairs"] = '<font color="#47a0f7">pairs</font>',
			})
		MainFrame.Options.Properties.Value.DisplayText.Text = Str
	end
end)
spawn(function()
	while wait() do
		if CurrentSelections == nil then
			TextValue.Text = "nil"
		else
			local Str = ""
			for _,v in pairs(CurrentSelections) do
				if Str == "" then
					Str = tostring(v.Name)
				else
					Str = Str .. ", " .. tostring(v.Name)
				end
			end
			TextValue.Text = Str
		end
	end
end)

ToolbarButton.Click:Connect(function() -- To Activate/Deactive The Plugin
	Plugin.Enabled = not Plugin.Enabled -- Sets The Enable To False/True
end)
```

Made for HiddenDevs programming event
