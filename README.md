-- MADE BY .antilua.
-- OPTIMIZED VERSION - UNIVERSAL PIANO SUPPORT
VERSION="1.9.5"
assert(isfolder and makefolder, "Unable to create folder")
if not isfolder("MIDI") then
	makefolder("MIDI")
	writefile("./MIDI/Summer.mid", game:HttpGetAsync("https://github.com/Ukrubojvo/api/raw/refs/heads/main/Summer.mid"))
end

if not isfolder("MIDI_Favorites") then
	makefolder("MIDI_Favorites")
end

if not shared.AntiLuaLoading then
	shared.AntiLuaLoading = true
else
	return "Already Loaded"
end

local run = function(func)
	xpcall(func, function(err)
		shared.AntiLuaLoading = false
		warn(err)
	end)
end

local WindUI
run(function()
	local ok, res = pcall(function()
		return require("./src/Init")
	end)
	if ok and res then
		WindUI = res
	else
		WindUI = loadstring(game:HttpGetAsync("https://github.com/Footagesus/WindUI/releases/latest/download/main.lua"))()
	end
end)

local function missing(t, f, fallback)
	if type(f) == t then return f end
	return fallback
end

local cloneref = missing("function", cloneref, function(...) return ... end)
local Services = setmetatable({}, {
	__index = function(self, name)
		self[name] = cloneref(game:GetService(name))
		return self[name]
	end
})

local RealGame    = game
local run_service = Services.RunService
local vim         = Services.VirtualInputManager
local uis         = Services.UserInputService
local players     = Services.Players
local player      = players.LocalPlayer

-- ============================================================
-- KEY MAP
-- ============================================================
local key_map = {
	[21]={keycode=Enum.KeyCode.One,ctrl=true},
	[22]={keycode=Enum.KeyCode.Two,ctrl=true},
	[23]={keycode=Enum.KeyCode.Three,ctrl=true},
	[24]={keycode=Enum.KeyCode.Four,ctrl=true},
	[25]={keycode=Enum.KeyCode.Five,ctrl=true},
	[26]={keycode=Enum.KeyCode.Six,ctrl=true},
	[27]={keycode=Enum.KeyCode.Seven,ctrl=true},
	[28]={keycode=Enum.KeyCode.Eight,ctrl=true},
	[29]={keycode=Enum.KeyCode.Nine,ctrl=true},
	[30]={keycode=Enum.KeyCode.Zero,ctrl=true},
	[31]={keycode=Enum.KeyCode.Q,ctrl=true},
	[32]={keycode=Enum.KeyCode.W,ctrl=true},
	[33]={keycode=Enum.KeyCode.E,ctrl=true},
	[34]={keycode=Enum.KeyCode.R,ctrl=true},
	[35]={keycode=Enum.KeyCode.T,ctrl=true},
	[36]={keycode=Enum.KeyCode.One,shift=false},[37]={keycode=Enum.KeyCode.One,shift=true},
	[38]={keycode=Enum.KeyCode.Two,shift=false},[39]={keycode=Enum.KeyCode.Two,shift=true},
	[40]={keycode=Enum.KeyCode.Three,shift=false},[41]={keycode=Enum.KeyCode.Four,shift=false},
	[42]={keycode=Enum.KeyCode.Four,shift=true},[43]={keycode=Enum.KeyCode.Five,shift=false},
	[44]={keycode=Enum.KeyCode.Five,shift=true},[45]={keycode=Enum.KeyCode.Six,shift=false},
	[46]={keycode=Enum.KeyCode.Six,shift=true},[47]={keycode=Enum.KeyCode.Seven,shift=false},
	[48]={keycode=Enum.KeyCode.Eight,shift=false},[49]={keycode=Enum.KeyCode.Eight,shift=true},
	[50]={keycode=Enum.KeyCode.Nine,shift=false},[51]={keycode=Enum.KeyCode.Nine,shift=true},
	[52]={keycode=Enum.KeyCode.Zero,shift=false},[53]={keycode=Enum.KeyCode.Q,shift=false},
	[54]={keycode=Enum.KeyCode.Q,shift=true},[55]={keycode=Enum.KeyCode.W,shift=false},
	[56]={keycode=Enum.KeyCode.W,shift=true},[57]={keycode=Enum.KeyCode.E,shift=false},
	[58]={keycode=Enum.KeyCode.E,shift=true},[59]={keycode=Enum.KeyCode.R,shift=false},
	[60]={keycode=Enum.KeyCode.T,shift=false},[61]={keycode=Enum.KeyCode.T,shift=true},
	[62]={keycode=Enum.KeyCode.Y,shift=false},[63]={keycode=Enum.KeyCode.Y,shift=true},
	[64]={keycode=Enum.KeyCode.U,shift=false},[65]={keycode=Enum.KeyCode.I,shift=false},
	[66]={keycode=Enum.KeyCode.I,shift=true},[67]={keycode=Enum.KeyCode.O,shift=false},
	[68]={keycode=Enum.KeyCode.O,shift=true},[69]={keycode=Enum.KeyCode.P,shift=false},
	[70]={keycode=Enum.KeyCode.P,shift=true},[71]={keycode=Enum.KeyCode.A,shift=false},
	[72]={keycode=Enum.KeyCode.S,shift=false},[73]={keycode=Enum.KeyCode.S,shift=true},
	[74]={keycode=Enum.KeyCode.D,shift=false},[75]={keycode=Enum.KeyCode.D,shift=true},
	[76]={keycode=Enum.KeyCode.F,shift=false},[77]={keycode=Enum.KeyCode.G,shift=false},
	[78]={keycode=Enum.KeyCode.G,shift=true},[79]={keycode=Enum.KeyCode.H,shift=false},
	[80]={keycode=Enum.KeyCode.H,shift=true},[81]={keycode=Enum.KeyCode.J,shift=false},
	[82]={keycode=Enum.KeyCode.J,shift=true},[83]={keycode=Enum.KeyCode.K,shift=false},
	[84]={keycode=Enum.KeyCode.L,shift=false},[85]={keycode=Enum.KeyCode.L,shift=true},
	[86]={keycode=Enum.KeyCode.Z,shift=false},[87]={keycode=Enum.KeyCode.Z,shift=true},
	[88]={keycode=Enum.KeyCode.X,shift=false},[89]={keycode=Enum.KeyCode.C,shift=false},
	[90]={keycode=Enum.KeyCode.C,shift=true},[91]={keycode=Enum.KeyCode.V,shift=false},
	[92]={keycode=Enum.KeyCode.V,shift=true},[93]={keycode=Enum.KeyCode.B,shift=false},
	[94]={keycode=Enum.KeyCode.B,shift=true},[95]={keycode=Enum.KeyCode.N,shift=false},
	[96]={keycode=Enum.KeyCode.M,shift=false},[97]={keycode=Enum.KeyCode.M,shift=true},
	[98]={keycode=Enum.KeyCode.U,ctrl=true},[99]={keycode=Enum.KeyCode.I,ctrl=true},
	[100]={keycode=Enum.KeyCode.O,ctrl=true},[101]={keycode=Enum.KeyCode.P,ctrl=true},
	[102]={keycode=Enum.KeyCode.A,ctrl=true},[103]={keycode=Enum.KeyCode.S,ctrl=true},
	[104]={keycode=Enum.KeyCode.D,ctrl=true},[105]={keycode=Enum.KeyCode.F,ctrl=true},
	[106]={keycode=Enum.KeyCode.G,ctrl=true},[107]={keycode=Enum.KeyCode.H,ctrl=true},
	[108]={keycode=Enum.KeyCode.J,ctrl=true},
}

-- ============================================================
-- VELOCITY SYSTEM
-- ============================================================
local pending_velocity      = 1.0
local last_note_press_time  = 0
local velocity_window       = 0.15
local sound_connections     = {}
local hooked_sounds         = {}
local auto_velocity_enabled = true

local function hook_sound(sound)
	if hooked_sounds[sound] then return end
	hooked_sounds[sound] = true
	local ok1, conn1 = pcall(function()
		return sound.Played:Connect(function()
			if not auto_velocity_enabled then return end
			if os.clock() - last_note_press_time <= velocity_window then
				pcall(function() sound.Volume = pending_velocity end)
			end
		end)
	end)
	if ok1 and conn1 then table.insert(sound_connections, conn1) end
	local ok2, conn2 = pcall(function()
		return sound:GetPropertyChangedSignal("Volume"):Connect(function()
			if not auto_velocity_enabled then return end
			if os.clock() - last_note_press_time <= velocity_window then
				local cur = sound.Volume
				if math.abs(cur - pending_velocity) > 0.01 and cur > 0.9 then
					pcall(function() sound.Volume = pending_velocity end)
				end
			end
		end)
	end)
	if ok2 and conn2 then table.insert(sound_connections, conn2) end
end

local function hook_all_sounds_in(obj)
	pcall(function()
		if obj:IsA("Sound") then hook_sound(obj) end
		for _, desc in ipairs(obj:GetDescendants()) do
			if desc:IsA("Sound") then hook_sound(desc) end
		end
	end)
end

local function setup_velocity_hooks()
	hook_all_sounds_in(workspace)
	pcall(function() hook_all_sounds_in(Services.SoundService) end)
	local ok1, c1 = pcall(function()
		return workspace.DescendantAdded:Connect(function(obj)
			if obj:IsA("Sound") then hook_sound(obj) end
		end)
	end)
	if ok1 and c1 then table.insert(sound_connections, c1) end
	local ok2, c2 = pcall(function()
		return Services.SoundService.DescendantAdded:Connect(function(obj)
			if obj:IsA("Sound") then hook_sound(obj) end
		end)
	end)
	if ok2 and c2 then table.insert(sound_connections, c2) end
	local ok3, c3 = pcall(function()
		return players.LocalPlayer.CharacterAdded:Connect(function(char)
			hook_all_sounds_in(char)
			char.DescendantAdded:Connect(function(obj)
				if obj:IsA("Sound") then hook_sound(obj) end
			end)
		end)
	end)
	if ok3 and c3 then table.insert(sound_connections, c3) end
end

local function set_pending_velocity(velocity_0_127)
	pending_velocity     = math.clamp(velocity_0_127 / 127, 0, 1)
	last_note_press_time = os.clock()
end

local function cleanup_velocity_hooks()
	for _, conn in ipairs(sound_connections) do
		pcall(function() conn:Disconnect() end)
	end
	sound_connections = {}
	hooked_sounds     = {}
end

-- ============================================================
-- PIANO CONTROLLER AUTO-DETECTION
-- ============================================================
local PianoController  = nil
local last_detect_time = 0
local detect_cooldown  = 3

local function is_valid_controller(t)
	if typeof(t) ~= "table" then return false end
	local has_press   = rawget(t,"PressClientKey") or rawget(t,"PressKey") or rawget(t,"PressNote") or rawget(t,"PlayNote")
	local has_release = rawget(t,"ReleaseClientKey") or rawget(t,"ReleaseKey") or rawget(t,"ReleaseNote") or rawget(t,"StopNote")
	return has_press ~= nil and has_release ~= nil
end

local function try_find_controller()
	local ok1, found1 = pcall(function()
		local pkg = Services.ReplicatedStorage:FindFirstChild("Packages")
		if not pkg then return nil end
		local knitMod = pkg:FindFirstChild("Knit")
		if not knitMod then return nil end
		local Knit = require(knitMod)
		for _, v in pairs(getgc(true)) do
			if typeof(v)=="table" and rawget(v,"Active")~=nil and rawget(v,"Piano")~=nil
				and rawget(v,"PressClientKey") and rawget(v,"ReleaseClientKey") then
				pcall(function()
					if v._modules and Knit.Modules and Knit.Modules.DefaultSettings then
						v._modules.DefaultSettings = require(Knit.Modules.DefaultSettings)
						v._settings        = v._modules.DefaultSettings
						v._VELOCITY_CURVES = v._settings.DEFAULT_VELOCITY_CURVES
					end
				end)
				return v
			end
		end
		return nil
	end)
	if ok1 and found1 and is_valid_controller(found1) then return found1 end
	local ok2, found2 = pcall(function()
		for _, v in pairs(getgc(true)) do
			if typeof(v)=="table" and rawget(v,"PressClientKey") and rawget(v,"ReleaseClientKey") then return v end
		end return nil
	end)
	if ok2 and found2 and is_valid_controller(found2) then return found2 end
	local ok3, found3 = pcall(function()
		for _, v in pairs(getgc(true)) do
			if typeof(v)=="table" and rawget(v,"PressKey") and rawget(v,"ReleaseKey") then return v end
		end return nil
	end)
	if ok3 and found3 and is_valid_controller(found3) then return found3 end
	local ok4, found4 = pcall(function()
		for _, v in pairs(getgc(true)) do
			if typeof(v)=="table" and rawget(v,"PressNote") and rawget(v,"ReleaseNote") then return v end
		end return nil
	end)
	if ok4 and found4 and is_valid_controller(found4) then return found4 end
	local ok5, found5 = pcall(function()
		for _, v in pairs(getgc(true)) do
			if typeof(v)=="table" and rawget(v,"PlayNote") and rawget(v,"StopNote") then return v end
		end return nil
	end)
	if ok5 and found5 and is_valid_controller(found5) then return found5 end
	return nil
end

local function auto_detect()
	if PianoController then return end
	local now = os.clock()
	if now - last_detect_time < detect_cooldown then return end
	last_detect_time = now
	local found = try_find_controller()
	if found then PianoController = found end
end

-- ============================================================
-- CONTROLLER VELOCITY
-- ============================================================
local function apply_controller_velocity(velocity_0_127)
	if not PianoController then return end
	local vol = velocity_0_127 / 127
	for _, name in ipairs({"SetVelocity","setVelocity","SetVolume","setVolume","SetNoteVelocity","setNoteVelocity"}) do
		if PianoController[name] then
			pcall(function() PianoController[name](PianoController, vol) end)
			pcall(function() PianoController[name](PianoController, velocity_0_127) end)
			return
		end
	end
	for _, prop in ipairs({"Velocity","velocity","Volume","volume"}) do
		if PianoController[prop] ~= nil then
			pcall(function() PianoController[prop] = vol end)
			return
		end
	end
end

-- ============================================================
-- MODIFIER KEY STATE
-- ============================================================
local vim_shift_held = false
local vim_ctrl_held  = false

local function vim_set_shift(state)
	if state == vim_shift_held then return end
	vim_shift_held = state
	pcall(function() vim:SendKeyEvent(state, Enum.KeyCode.LeftShift, false, RealGame) end)
end

local function vim_set_ctrl(state)
	if state == vim_ctrl_held then return end
	vim_ctrl_held = state
	pcall(function() vim:SendKeyEvent(state, Enum.KeyCode.LeftControl, false, RealGame) end)
end

local function vim_release_all_modifiers()
	if vim_shift_held then
		vim_shift_held = false
		pcall(function() vim:SendKeyEvent(false, Enum.KeyCode.LeftShift, false, RealGame) end)
	end
	if vim_ctrl_held then
		vim_ctrl_held = false
		pcall(function() vim:SendKeyEvent(false, Enum.KeyCode.LeftControl, false, RealGame) end)
	end
end

-- ============================================================
-- SUSTAIN
-- ============================================================
local function apply_sustain_to_controller(state)
	if PianoController then
		for _, name in ipairs({"ToggleSustain","SetSustain","setSustain","Sustain","toggleSustain"}) do
			if PianoController[name] then
				pcall(function() PianoController[name](PianoController, state) end)
				return
			end
		end
		for _, prop in ipairs({"Sustain","sustain","SustainPedal","sustainPedal"}) do
			if PianoController[prop] ~= nil then
				pcall(function() PianoController[prop] = state end)
				return
			end
		end
	end
	pcall(function() vim:SendKeyEvent(state, Enum.KeyCode.Space, false, RealGame) end)
end

-- ============================================================
-- NOTE PRESS / RELEASE
-- ============================================================
local function press_note(note_number, velocity_0_127, key_entry)
	if not key_entry then return end
	local kc     = key_entry.keycode
	local kc_val = (typeof(kc) == "EnumItem") and kc.Value or kc
	if auto_velocity_enabled then
		set_pending_velocity(velocity_0_127)
		apply_controller_velocity(velocity_0_127)
	end
	if PianoController then
		if PianoController.PressClientKey then
			pcall(function() PianoController:PressClientKey(kc_val, note_number-20, nil, nil, nil, nil, nil) end)
			return
		end
		if PianoController.PressKey then
			pcall(function() PianoController:PressKey(kc_val) end) return
		end
		if PianoController.PressNote then
			pcall(function() PianoController:PressNote(note_number) end) return
		end
		if PianoController.PlayNote then
			pcall(function() PianoController:PlayNote(note_number, velocity_0_127) end) return
		end
	end
	vim_set_ctrl(key_entry.ctrl == true)
	vim_set_shift(key_entry.shift == true)
	pcall(function() vim:SendKeyEvent(true, kc, false, RealGame) end)
end

local function release_note(note_number, key_entry)
	if not key_entry then return end
	local kc     = key_entry.keycode
	local kc_val = (typeof(kc) == "EnumItem") and kc.Value or kc
	if PianoController then
		if PianoController.ReleaseClientKey then
			pcall(function() PianoController:ReleaseClientKey(kc_val) end) return
		end
		if PianoController.ReleaseKey then
			pcall(function() PianoController:ReleaseKey(kc_val) end) return
		end
		if PianoController.ReleaseNote then
			pcall(function() PianoController:ReleaseNote(note_number) end) return
		end
		if PianoController.StopNote then
			pcall(function() PianoController:StopNote(note_number) end) return
		end
	end
	pcall(function() vim:SendKeyEvent(false, kc, false, RealGame) end)
end

-- ============================================================
-- PLAYBACK STATE
-- ============================================================
local events              = {}
local tempo_events        = {}
local sustain_active      = false
local sustain_midi_state  = false
local key88_enabled       = true
local auto_sustain_enabled   = true
local no_note_off_enabled    = false
local force_note_off_sustain = true
local random_note_enabled    = false
local deblack_enabled     = false
local deblack_level       = 65
local deblack_strict      = true
local FastNoteOff         = false
local FastNoteOffDelay    = 0.02
local active_notes        = {}
local start_time          = 0
local next_event_index    = 1
local is_paused           = true
local pause_position      = 0
local song_started        = false
local total_duration      = 0
local midi_files          = {}
local favorites_files     = {}
local playback_speed      = 1.0
local midi_loaded         = false
local is_loading          = false
local current_loaded_file        = nil
local current_loaded_is_favorite = false
local notifications_enabled      = true
local hand_mode       = "Both"
local hand_split_note = 60
local MIDI_FOLDER      = "./MIDI"
local FAVORITES_FOLDER = "./MIDI_Favorites"

-- Skip amount in seconds
local skip_amount = 5

-- ============================================================
-- UI HIDE / SHOW STATE
-- ============================================================
local f11_countdown_active = false
local f11_start_delay      = 3
local ui_is_hidden         = false
local main_screen_gui      = nil

local function hide_ui()
	if ui_is_hidden then return end
	if not main_screen_gui then return end
	ui_is_hidden = true
	pcall(function() main_screen_gui.Enabled = false end)
end

local function show_ui()
	if not ui_is_hidden then return end
	if not main_screen_gui then return end
	ui_is_hidden = false
	pcall(function() main_screen_gui.Enabled = true end)
end

-- ============================================================
-- TIME FORMAT HELPER
-- ============================================================
local function format_time(seconds)
	if not seconds or seconds < 0 then seconds = 0 end
	local mins = math.floor(seconds / 60)
	local secs = math.floor(seconds % 60)
	return string.format("%d:%02d", mins, secs)
end

-- ============================================================
-- SUSTAIN MANAGEMENT
-- ============================================================
local function set_sustain(state)
	if sustain_active == state then return end
	apply_sustain_to_controller(state)
	sustain_active = state
end

local function get_target_sustain()
	if not auto_sustain_enabled then return true end
	if no_note_off_enabled and force_note_off_sustain then return true end
	return sustain_midi_state
end

local function apply_midi_sustain(value)
	sustain_midi_state = value >= 64
	set_sustain(get_target_sustain())
end

local function init_sustain()
	sustain_midi_state = false
	sustain_active     = false
	set_sustain(get_target_sustain())
end

local function refresh_sustain()
	local target = get_target_sustain()
	sustain_active = not target
	set_sustain(target)
end

-- ============================================================
-- HELPERS
-- ============================================================
local function notify(title, content, duration, icon)
	if notifications_enabled and WindUI then
		WindUI:Notify({Title=title or "MidiPlayer", Content=content or "", Duration=duration or 3, Icon=icon or "bird"})
	end
end

local function should_play_note(note_number)
	if hand_mode == "Both"  then return true end
	if hand_mode == "Right" then return note_number >= hand_split_note end
	if hand_mode == "Left"  then return note_number <  hand_split_note end
	return true
end

local function get_note_name(n)
	local names = {"C","C#","D","D#","E","F","F#","G","G#","A","A#","B"}
	return names[(n%12)+1]..tostring(math.floor(n/12)-1)
end

local function get_playback_pos()
	if is_paused then return pause_position end
	return (os.clock() - start_time) * playback_speed
end

-- ============================================================
-- RELEASE ALL
-- ============================================================
local function release_all_keys()
	for note, data in pairs(active_notes) do
		if data.key_entry then release_note(note, data.key_entry) end
	end
	active_notes = {}
	vim_release_all_modifiers()
end

-- ============================================================
-- PLAYBACK CONTROL
-- ============================================================
local function begin_playback()
	auto_detect()
	start_time   = os.clock() - (pause_position / playback_speed)
	is_paused    = false
	song_started = true
	release_all_keys()
	init_sustain()
end

local function pause_playback()
	if is_paused then return end
	pause_position = get_playback_pos()
	is_paused      = true
	release_all_keys()
end

local function resume_playback()
	if not is_paused then return end
	start_time = os.clock() - (pause_position / playback_speed)
	is_paused  = false
	refresh_sustain()
end

local function toggle_pause_resume()
	if is_paused then resume_playback() else pause_playback() end
end

local function stop_playback()
	is_paused        = true
	pause_position   = 0
	next_event_index = 1
	start_time       = 0
	song_started     = false
	release_all_keys()
	set_sustain(false)
	sustain_midi_state = false
end

-- ============================================================
-- SEEK — silently scans events without playing any notes
-- ============================================================
local function seek_to(ratio)
	ratio = math.clamp(ratio, 0, 1)
	local target = total_duration * ratio

	release_all_keys()

	local last_sus  = 0
	local new_index = #events + 1

	for i = 1, #events do
		local ev = events[i]
		if ev.abs_time > target then
			new_index = i
			break
		end
		if ev.type == "control" then
			last_sus = ev.vel
		end
	end

	pause_position   = target
	next_event_index = new_index
	apply_midi_sustain(last_sus)

	if not is_paused then
		start_time = os.clock() - (target / playback_speed)
	end
end

-- ============================================================
-- SKIP FORWARD / BACKWARD
-- ============================================================
local function skip_by(seconds)
	if not midi_loaded or total_duration == 0 then return end
	local current = get_playback_pos()
	local target  = math.clamp(current + seconds, 0, total_duration)
	seek_to(target / total_duration)
end

-- ============================================================
-- REALTIME PLAYBACK LOOP
-- ============================================================
local function play_realtime()
	if is_paused then return end
	auto_detect()

	local elapsed = get_playback_pos()

	while next_event_index <= #events do
		local ev      = events[next_event_index]
		local ev_time = ev.abs_time

		if ev.type == "off" and FastNoteOff then
			ev_time = ev_time - FastNoteOffDelay
			if ev_time < 0 then ev_time = 0 end
		end
		if ev.type == "on" and random_note_enabled then
			ev_time = ev_time - (math.random(0,5) * 0.01)
			if ev_time < 0 then ev_time = 0 end
		end

		if ev_time > elapsed then break end

		if ev.type == "on" then
			if should_play_note(ev.note) then
				local k = key_map[ev.note]
				if k and not (not key88_enabled and k.ctrl == true) then
					press_note(ev.note, ev.vel, k)
					active_notes[ev.note] = {key_entry = k}
					if no_note_off_enabled then
						release_note(ev.note, k)
						active_notes[ev.note] = nil
					end
				end
			end
		elseif ev.type == "off" then
			if should_play_note(ev.note) and not no_note_off_enabled then
				local nd = active_notes[ev.note]
				if nd then
					release_note(ev.note, nd.key_entry)
					active_notes[ev.note] = nil
				end
			end
		elseif ev.type == "control" then
			apply_midi_sustain(ev.vel)
		end

		next_event_index = next_event_index + 1
	end

	if next_event_index > #events then
		stop_playback()
	end
end

-- ============================================================
-- FILE HELPERS
-- ============================================================
local function get_regular_midi_files()
	local out = {}
	local ok, files = pcall(listfiles, MIDI_FOLDER)
	if ok and files then
		for _, f in ipairs(files) do
			if f:match("%.midi?$") then table.insert(out, f:match("[^/\\]+$")) end
		end
	end
	return out
end

local function get_favorites_files()
	local out = {}
	local ok, files = pcall(listfiles, FAVORITES_FOLDER)
	if ok and files then
		for _, f in ipairs(files) do
			if f:match("%.midi?$") then table.insert(out, f:match("[^/\\]+$")) end
		end
	end
	return out
end

local function move_to_favorites(filename)
	local src = MIDI_FOLDER.."/"..filename
	local dst = FAVORITES_FOLDER.."/"..filename
	if not isfile(src) then return false,"Source file not found" end
	if isfile(dst)  then return false,"Already in favorites" end
	local ok, data = pcall(readfile, src)
	if not ok then return false,"Failed to read" end
	if not pcall(writefile, dst, data) then return false,"Failed to write" end
	pcall(delfile, src)
	return true,"Moved to favorites"
end

local function move_from_favorites(filename)
	local src = FAVORITES_FOLDER.."/"..filename
	local dst = MIDI_FOLDER.."/"..filename
	if not isfile(src) then return false,"Source not found" end
	if isfile(dst)  then return false,"Already in MIDI folder" end
	local ok, data = pcall(readfile, src)
	if not ok then return false,"Failed to read" end
	if not pcall(writefile, dst, data) then return false,"Failed to write" end
	pcall(delfile, src)
	return true,"Moved back"
end

-- ============================================================
-- MIDI PARSER
-- ============================================================
local function read_var_int(data, offset)
	local value, bytes_read = 0, 0
	while true do
		local byte = string.byte(data, offset + bytes_read)
		if not byte then break end
		bytes_read = bytes_read + 1
		value = bit32.bor(bit32.lshift(value,7), bit32.band(byte,127))
		if bit32.band(byte,128) == 0 then break end
	end
	return value, bytes_read
end

local function calc_realtime(ticks, tpb, tempo_changes)
	local cur_tick, cur_ms, cur_tempo = 0, 0, 500000
	for _, te in ipairs(tempo_changes) do
		if te.tick <= ticks then
			cur_ms    = cur_ms + ((te.tick - cur_tick) * cur_tempo / 1000) / tpb
			cur_tick  = te.tick
			cur_tempo = te.tempo
		else break end
	end
	return (cur_ms + ((ticks - cur_tick) * cur_tempo / 1000) / tpb) / 1000
end

local function apply_deblack(parsed)
	if not deblack_enabled then return parsed end
	local note_on_times, last_note_off, keep = {}, {}, {}
	for i, ev in ipairs(parsed) do
		if not ev.abs_time or ev.type=="control" or not ev.channel or not ev.note then
			keep[i] = true
		else
			local key = ev.channel..":"..ev.note
			if ev.vel and ev.vel > 0 then
				local ignore = false
				if deblack_strict then
					local prev = last_note_off[key]
					if prev and (ev.abs_time-prev.t)<0.035 and math.abs(ev.vel-prev.v)<7 then
						ignore = true
					end
				end
				if not ignore then note_on_times[key]={t=ev.abs_time,idx=i,v=ev.vel} end
			else
				local on = note_on_times[key]
				if on then
					last_note_off[key]={t=ev.abs_time,v=on.v}
					note_on_times[key]=nil
					if not (on.v<=deblack_level and (ev.abs_time-on.t)*1000<20) then
						keep[on.idx]=true; keep[i]=true
					end
				else keep[i]=true end
			end
		end
	end
	local out,n={},0
	for i,ev in ipairs(parsed) do
		if keep[i] then n=n+1; out[n]=ev end
	end
	return out
end

local function parse_midi(data)
	local buf=data; local offset=1; local track_end=0
	local header_done=false; local tpb=480; local last_status=nil
	local track_time=0; local on_stack={}; local parsed={}
	local tempo_changes={{tick=0,tempo=500000}}; local n=0
	local last_yield=os.clock()
	while true do
		if os.clock()-last_yield>0.033 then task.wait(); last_yield=os.clock() end
		if not header_done then
			if #buf<14 then break end
			if buf:sub(1,4)~="MThd" then break end
			tpb=string.unpack(">H",buf,13); offset=15; header_done=true
		end
		if offset>=track_end then
			if #buf-offset+1<8 then break end
			if buf:sub(offset,offset+3)~="MTrk" then break end
			offset=offset+4
			local tlen=string.unpack(">I4",buf,offset)
			offset=offset+4; track_end=offset+tlen-1
			last_status=nil; track_time=0; on_stack={}
		end
		if offset>track_end then break end
		local delta,db=read_var_int(buf,offset)
		offset=offset+db; track_time=track_time+delta
		local sb=string.byte(buf,offset)
		if not sb then break end
		local status
		if bit32.band(sb,128)~=0 then
			last_status=sb; status=sb; offset=offset+1
		else
			if not last_status then break end; status=last_status
		end
		local cmd=bit32.band(status,240); local ch=bit32.band(status,15)
		if cmd==144 or cmd==128 then
			local nn=string.byte(buf,offset); local vel=string.byte(buf,offset+1)
			if not nn or not vel then break end
			offset=offset+2
			local is_on=cmd==144 and vel>0; local k=nn..":"..ch
			if is_on then
				if on_stack[k] then
					local p=on_stack[k]
					local on_t=calc_realtime(p.on_tick,tpb,tempo_changes)
					local off_t=calc_realtime(track_time,tpb,tempo_changes)
					n=n+1; parsed[n]={type="on",note=p.note_name,vel=p.velocity,channel=p.channel,abs_time=on_t}
					n=n+1; parsed[n]={type="off",note=p.note_name,channel=p.channel,abs_time=off_t}
					on_stack[k]=nil
				end
				on_stack[k]={on_tick=track_time,velocity=vel,note_name=nn,channel=ch}
			else
				local p=on_stack[k]
				if p then
					local on_t=calc_realtime(p.on_tick,tpb,tempo_changes)
					local off_t=calc_realtime(track_time,tpb,tempo_changes)
					n=n+1; parsed[n]={type="on",note=p.note_name,vel=p.velocity,channel=p.channel,abs_time=on_t}
					n=n+1; parsed[n]={type="off",note=p.note_name,channel=p.channel,abs_time=off_t}
					on_stack[k]=nil
				end
			end
		elseif cmd==176 then
			local ct=string.byte(buf,offset); local cv=string.byte(buf,offset+1)
			if not ct or not cv then break end
			offset=offset+2
			if ct==64 then
				n=n+1; parsed[n]={type="control",vel=cv,abs_time=calc_realtime(track_time,tpb,tempo_changes)}
			end
		elseif status==255 then
			local mt=string.byte(buf,offset)
			if not mt then break end
			offset=offset+1
			local mlen,mlb=read_var_int(buf,offset); offset=offset+mlb
			if mt==81 and mlen==3 then
				local b1,b2,b3=string.byte(buf,offset,offset+2)
				if b1 and b2 and b3 then
					table.insert(tempo_changes,{tick=track_time,tempo=b1*65536+b2*256+b3})
				end
			end
			offset=offset+mlen
		else
			offset=offset+((cmd==192 or cmd==208) and 1 or 2)
		end
		if offset>track_end then offset=track_end+1 end
	end
	for _,p in pairs(on_stack) do
		n=n+1; parsed[n]={type="on",note=p.note_name,vel=p.velocity,channel=p.channel,abs_time=calc_realtime(p.on_tick,tpb,tempo_changes)}
	end
	task.wait()
	table.sort(parsed,function(a,b) return a.abs_time<b.abs_time end)
	task.wait()
	return apply_deblack(parsed),tempo_changes
end

-- ============================================================
-- LOAD MIDI
-- ============================================================
local function load_midi_from_data(data, status_fn, filename, is_fav)
	if is_loading then return end
	is_loading = true
	status_fn("Parsing MIDI...")
	task.spawn(function()
		local ok, parsed, tc = pcall(parse_midi, data)
		if not ok or not parsed then
			status_fn("Parse error"); is_loading=false; return
		end
		events         = parsed
		tempo_events   = tc or {}
		total_duration = (events[#events] and events[#events].abs_time) or 0
		next_event_index = 1
		pause_position   = 0
		is_paused        = true
		song_started     = false
		midi_loaded      = true
		is_loading       = false
		current_loaded_file        = filename
		current_loaded_is_favorite = is_fav or false
		status_fn("Loaded "..#events.." events ("..string.format("%.2f",total_duration).."s) — Press Play or F11")
	end)
end

local function auto_load_file(filename, is_fav, status_fn)
	if is_loading then status_fn("Already loading...") return end
	local path = (is_fav and FAVORITES_FOLDER or MIDI_FOLDER).."/"..filename
	if not isfile(path) then status_fn("File not found: "..filename) return end
	stop_playback()
	local ok, data = pcall(readfile, path)
	if ok and data then
		status_fn("Loading: "..filename)
		load_midi_from_data(data, status_fn, filename, is_fav)
	else
		status_fn("Failed to read: "..filename)
	end
end

-- ============================================================
-- UI REFS
-- ============================================================
local file_dropdown_ref
local fav_dropdown_ref
local fav_status_ref
local status_label_ref
local hand_status_ref
local timer_paragraph_ref = nil
local seek_slider_ref     = nil

local function refresh_dropdowns()
	midi_files      = get_regular_midi_files()
	favorites_files = get_favorites_files()
	if file_dropdown_ref then pcall(function() file_dropdown_ref:Refresh(midi_files) end) end
	if fav_dropdown_ref  then pcall(function() fav_dropdown_ref:Refresh(favorites_files) end) end
	if fav_status_ref    then pcall(function() fav_status_ref:SetDesc(#favorites_files.." favorites") end) end
end

local function update_hand_status()
	if not hand_status_ref then return end
	local t = "Playing: Both Hands"
	if hand_mode=="Right" then t="Right Hand (Notes >= "..get_note_name(hand_split_note)..")"
	elseif hand_mode=="Left" then t="Left Hand (Notes < "..get_note_name(hand_split_note)..")" end
	hand_status_ref:SetDesc(t)
end

-- ============================================================
-- TIMER + SEEK SLIDER UPDATER
-- ============================================================
local last_timer_text    = ""
local ignore_seek_update = false

local function update_timer_and_seek()
	local pos   = get_playback_pos()
	local total = total_duration or 0

	if timer_paragraph_ref then
		local dot
		if not is_paused and song_started then
			dot = math.floor(os.clock() * 2) % 2 == 0 and "🔴" or "🟥"
		else
			dot = "⚫"
		end
		local text = dot .. " " .. format_time(pos) .. " / " .. format_time(total)
		if text ~= last_timer_text then
			last_timer_text = text
			pcall(function() timer_paragraph_ref:SetDesc(text) end)
		end
	end

	if seek_slider_ref and not ignore_seek_update and total > 0 then
		pcall(function()
			ignore_seek_update = true
			seek_slider_ref:Set(math.clamp(pos, 0, total))
			ignore_seek_update = false
		end)
	end
end

local function handle_play(status_fn)
	if not midi_loaded or #events == 0 then
		status_fn("No MIDI loaded — select a file first") return
	end
	if not song_started then
		next_event_index = 1
		pause_position   = 0
		begin_playback()
		status_fn("Playing: "..(current_loaded_file or ""))
	elseif is_paused then
		resume_playback()
		status_fn("Resumed at "..string.format("%.2f", pause_position).."s")
	else
		status_fn("Already playing")
	end
end

-- ============================================================
-- F11
-- ============================================================
local function f11_play_with_countdown()
	if f11_countdown_active then return end
	if not midi_loaded or #events == 0 then
		notify("MIDI Player", "No MIDI loaded — select a file first", 3, "bird")
		if status_label_ref then status_label_ref:SetDesc("No MIDI loaded — select a file first") end
		return
	end
	f11_countdown_active = true
	stop_playback()
	local file_name = current_loaded_file or "Unknown"
	hide_ui()
	notify("MIDI Player","Music starts in "..f11_start_delay.."s — "..file_name.."\nF12 = Show UI",f11_start_delay+1,"clock")
	task.spawn(function()
		task.wait(f11_start_delay)
		next_event_index = 1; pause_position = 0
		begin_playback()
		notify("MIDI Player","Now playing: "..file_name.."\nPress F12 to show UI",4,"music")
		if status_label_ref then status_label_ref:SetDesc("Playing: "..file_name.." (F12 to show UI)") end
		f11_countdown_active = false
	end)
end

-- ============================================================
-- F12
-- ============================================================
local function f12_show_ui()
	show_ui()
	notify("MIDI Player","UI restored",2,"bird")
	if status_label_ref then
		local desc = "UI restored"
		if not is_paused and current_loaded_file then
			desc = desc.." — Playing: "..current_loaded_file
		elseif is_paused and song_started and current_loaded_file then
			desc = desc.." — Paused: "..current_loaded_file
		end
		status_label_ref:SetDesc(desc)
	end
end

-- ============================================================
-- FIND SCREENGUI
-- ============================================================
local function extract_screengui_from_window(win)
	if not win then return nil end
	local function climb(inst)
		local cur = inst
		for _ = 1, 20 do
			if not cur then break end
			if pcall(function() return cur:IsA("ScreenGui") end) and cur:IsA("ScreenGui") then return cur end
			local ok, parent = pcall(function() return cur.Parent end)
			if not ok then break end
			cur = parent
		end
		return nil
	end
	local known_keys = {"ScreenGui","_screenGui","GUI","_gui","Gui","MainFrame","_mainFrame","Frame","_frame","Container","_container","Root","_root"}
	for _, key in ipairs(known_keys) do
		local ok, val = pcall(function() return rawget(win, key) end)
		if ok and typeof(val) == "Instance" then
			local sg = climb(val); if sg then return sg end
		end
	end
	local ok2, items = pcall(function()
		local t = {}; for k, v in pairs(win) do t[#t+1] = v end; return t
	end)
	if ok2 and items then
		for _, val in ipairs(items) do
			if typeof(val) == "Instance" then
				local sg = climb(val); if sg then return sg end
			end
		end
	end
	local function check_container(container)
		if not container then return nil end
		local ok3, c = pcall(function() return container:GetChildren() end)
		if not ok3 then return nil end
		local roblox_names = {RobloxGui=true,Chat=true,BubbleChat=true,PlayerList=true,RobloxPromptGui=true,TopBarApp=true,RobloxNotificationGui=true,RobloxLoadingGui=true,PurchasePromptApp=true,RobloxNetworkPauseNotification=true}
		for _, child in ipairs(c) do
			local is_sg = pcall(function() return child:IsA("ScreenGui") end) and child:IsA("ScreenGui")
			if is_sg and not roblox_names[child.Name] then return child end
		end
		return nil
	end
	local sg
	pcall(function() if gethui then sg = check_container(gethui()) end end)
	if sg then return sg end
	pcall(function() sg = check_container(Services.CoreGui) end)
	if sg then return sg end
	pcall(function() sg = check_container(player:FindFirstChildOfClass("PlayerGui")) end)
	return sg
end

-- ============================================================
-- UI
-- ============================================================
run(function()
	task.spawn(function() setup_velocity_hooks() end)
	task.spawn(function() task.wait(1); try_find_controller() end)

	midi_files      = get_regular_midi_files()
	favorites_files = get_favorites_files()

	-- Window — no Title emoji, no Author, no User avatar block
	local Window = WindUI:CreateWindow({
		Title         = "MIDI Player",
		Size          = UDim2.fromOffset(580, 260),
		SideBarWidth  = 150,
		Folder        = "AntiLua",
		NewElements   = true,
		HideSearchBar = true,
	})

	main_screen_gui = extract_screengui_from_window(Window)
	if not main_screen_gui then
		task.spawn(function()
			task.wait(0.1)
			if not main_screen_gui then main_screen_gui = extract_screengui_from_window(Window) end
		end)
	end

	Window:Tag({Title="v"..VERSION, Icon="github", Color=Color3.fromHex("#1c1c1c")})
	Window:OnDestroy(function()
		shared.AntiLuaLoading = false
		stop_playback(); release_all_keys(); cleanup_velocity_hooks()
		events = {}; midi_loaded = false; is_loading = false
		main_screen_gui = nil; timer_paragraph_ref = nil; seek_slider_ref = nil
	end)

	local MainTab     = Window:Tab({Title="Main",     Icon="house"})
	local FavTab      = Window:Tab({Title="Favorites", Icon="star"})
	local HandsTab    = Window:Tab({Title="Hands",     Icon="hand"})
	local SettingsTab = Window:Tab({Title="Settings",  Icon="settings"})

	-- ── MAIN TAB ─────────────────────────────────────────────

	local status_label = MainTab:Paragraph({
		Title = "Status",
		Desc  = "Ready — Select a song then press Play or F11"
	})
	status_label_ref = status_label

	-- Timer paragraph
	local timer_paragraph = MainTab:Paragraph({
		Title = "Playback Time",
		Desc  = "⚫ 0:00 / 0:00"
	})
	timer_paragraph_ref = timer_paragraph

	-- Seek / progress bar
	local seek_was_playing = false
	local seek_commit_time = 0

	seek_slider_ref = MainTab:Slider({
		Title    = "Song Position",
		Desc     = "Drag to seek anywhere in the song",
		Step     = 0.1,
		Value    = {Min=0, Max=1, Default=0},
		Callback = function(v)
			if ignore_seek_update then return end
			if not midi_loaded or total_duration == 0 then return end
			if not is_paused and not seek_was_playing then
				seek_was_playing = true
				pause_playback()
			end
			seek_commit_time = os.clock()
			seek_to(v / total_duration)
			local my_commit = seek_commit_time
			task.spawn(function()
				task.wait(0.35)
				if seek_commit_time == my_commit and seek_was_playing then
					seek_was_playing = false
					resume_playback()
				end
			end)
		end
	})

	-- Skip buttons
	MainTab:Button({
		Title    = "Skip Back -"..skip_amount.."s",
		Callback = function()
			skip_by(-skip_amount)
			if status_label_ref then
				status_label_ref:SetDesc("Skipped back — "..format_time(get_playback_pos()).." / "..format_time(total_duration))
			end
		end
	})

	MainTab:Button({
		Title    = "Skip Forward +"..skip_amount.."s",
		Callback = function()
			skip_by(skip_amount)
			if status_label_ref then
				status_label_ref:SetDesc("Skipped forward — "..format_time(get_playback_pos()).." / "..format_time(total_duration))
			end
		end
	})

	MainTab:Slider({
		Title    = "Skip Amount (seconds)",
		Step     = 1,
		Value    = {Min=1, Max=60, Default=5},
		Callback = function(v)
			skip_amount = math.floor(v)
		end
	})

	MainTab:Paragraph({
		Title = "Hotkeys",
		Desc  = "F11 — Hide UI + Auto-play after delay\nF12 — Show UI instantly"
	})

	local selected_file = nil
	file_dropdown_ref = MainTab:Dropdown({
		Title    = "MIDI Files",
		Desc     = "Select to auto-load",
		Values   = midi_files,
		Value    = nil,
		Callback = function(option)
			selected_file = option
			auto_load_file(option, false, function(msg)
				status_label:SetDesc(msg)
				task.spawn(function()
					task.wait(0.5)
					if seek_slider_ref and total_duration > 0 then
						pcall(function()
							seek_slider_ref:SetMin(0)
							seek_slider_ref:SetMax(total_duration)
						end)
					end
				end)
			end)
		end
	})

	MainTab:Button({
		Title    = "Refresh File List",
		Callback = function()
			refresh_dropdowns()
			status_label:SetDesc(#midi_files.." files, "..#favorites_files.." favorites")
		end
	})

	MainTab:Button({
		Title    = "Move to Favorites",
		Callback = function()
			local f = (current_loaded_file ~= "" and not current_loaded_is_favorite and current_loaded_file) or selected_file
			if not f then
				status_label:SetDesc(current_loaded_is_favorite and "Already a favorite" or "No file selected")
				return
			end
			local ok, msg = move_to_favorites(f)
			status_label:SetDesc(ok and msg..": "..f or msg)
			if ok then
				if current_loaded_file == f then current_loaded_is_favorite = true end
				selected_file = nil; refresh_dropdowns()
				notify("MIDI Player","Saved to Favorites: "..f,3,"star")
			end
		end
	})

	MainTab:Button({
		Title    = "Play",
		Callback = function() handle_play(function(msg) status_label:SetDesc(msg) end) end
	})

	MainTab:Button({
		Title    = "Pause / Resume",
		Callback = function()
			if not midi_loaded or #events == 0 then status_label:SetDesc("No MIDI loaded") return end
			if not song_started then handle_play(function(msg) status_label:SetDesc(msg) end) return end
			toggle_pause_resume()
			status_label:SetDesc(is_paused
				and "Paused at "..string.format("%.2f",pause_position).."s"
				or  "Resumed at "..string.format("%.2f",pause_position).."s")
		end
	})

	MainTab:Button({
		Title    = "Stop",
		Callback = function()
			stop_playback()
			status_label:SetDesc("Stopped — Press Play or F11 to restart")
		end
	})

	MainTab:Button({
		Title    = "F11 — Quick Play (hide UI + delay + play)",
		Desc     = "Hides UI instantly, waits delay, then plays. F12 to show UI.",
		Callback = function() f11_play_with_countdown() end
	})

	local ignore_speed    = false
	local was_playing_spd = false
	local last_speed_t    = 0
	local speed_slider_ref
	speed_slider_ref = MainTab:Slider({
		Title    = "Playback Speed (%)",
		Step     = 0.1,
		Value    = {Min=50, Max=200, Default=100},
		Callback = function(v)
			if ignore_speed then return end
			if not is_paused and not was_playing_spd then was_playing_spd = true; pause_playback() end
			last_speed_t = os.clock()
			local old = get_playback_pos()
			playback_speed = math.clamp(v/100, 0.5, 2.0)
			pause_position = old
			task.spawn(function()
				task.wait(0.3)
				if os.clock()-last_speed_t >= 0.3 and was_playing_spd then
					resume_playback(); was_playing_spd = false
				end
			end)
		end
	})

	-- ── FAVORITES TAB ────────────────────────────────────────
	fav_status_ref = FavTab:Paragraph({Title="Favorites", Desc=#favorites_files.." favorites"})

	local selected_fav = nil
	fav_dropdown_ref = FavTab:Dropdown({
		Title    = "Your Favorites",
		Desc     = "Select to auto-load",
		Values   = favorites_files,
		Value    = nil,
		Callback = function(option)
			selected_fav = option
			auto_load_file(option, true, function(msg)
				fav_status_ref:SetDesc(msg); status_label:SetDesc(msg)
				task.spawn(function()
					task.wait(0.5)
					if seek_slider_ref and total_duration > 0 then
						pcall(function()
							seek_slider_ref:SetMin(0)
							seek_slider_ref:SetMax(total_duration)
						end)
					end
				end)
			end)
		end
	})

	FavTab:Button({Title="Play", Callback=function()
		handle_play(function(msg) fav_status_ref:SetDesc(msg); status_label:SetDesc(msg) end)
	end})

	FavTab:Button({
		Title="F11 — Quick Play (hide UI + delay + play)",
		Desc="Hides UI instantly, waits delay, then plays. F12 to show UI.",
		Callback=function() f11_play_with_countdown() end
	})

	FavTab:Button({Title="Pause / Resume", Callback=function()
		if not midi_loaded or #events==0 then fav_status_ref:SetDesc("No MIDI loaded") return end
		if not song_started then handle_play(function(msg) fav_status_ref:SetDesc(msg); status_label:SetDesc(msg) end) return end
		toggle_pause_resume()
		local msg = is_paused and "Paused at "..string.format("%.2f",pause_position).."s"
			or "Resumed at "..string.format("%.2f",pause_position).."s"
		fav_status_ref:SetDesc(msg); status_label:SetDesc(msg)
	end})

	FavTab:Button({Title="Stop", Callback=function()
		stop_playback(); fav_status_ref:SetDesc("Stopped"); status_label:SetDesc("Stopped")
	end})

	FavTab:Button({Title="Remove from Favorites", Callback=function()
		if not selected_fav then fav_status_ref:SetDesc("None selected") return end
		local ok, msg = move_from_favorites(selected_fav)
		fav_status_ref:SetDesc(ok and "Removed: "..selected_fav or msg)
		if ok then
			if current_loaded_file==selected_fav then current_loaded_is_favorite=false end
			selected_fav=nil; refresh_dropdowns()
		end
	end})

	FavTab:Button({Title="Refresh", Callback=function()
		refresh_dropdowns(); fav_status_ref:SetDesc(#favorites_files.." favorites")
	end})

	-- ── HANDS TAB ────────────────────────────────────────────
	hand_status_ref = HandsTab:Paragraph({Title="Hand Selection", Desc="Playing: Both Hands"})
	HandsTab:Dropdown({
		Title="Hand Mode", Desc="Which hand to play",
		Values={"Both","Right","Left"}, Value="Both",
		Callback=function(v)
			hand_mode=v; update_hand_status(); release_all_keys()
			notify("MIDI Player","Hand Mode: "..v,3,"hand")
		end
	})

	-- ── SETTINGS TAB ─────────────────────────────────────────
	SettingsTab:Toggle({Title="Notifications", Desc="Enable notifications", Value=true,
		Callback=function(v) notifications_enabled=v end})

	SettingsTab:Toggle({Title="DeBlack", Desc="Filter ghost notes (reload after change)", Value=false,
		Callback=function(v) deblack_enabled=v end})

	SettingsTab:Slider({Title="DeBlack Level", Desc="Max velocity to filter (0-127)", Step=1,
		Value={Min=0,Max=127,Default=65}, Callback=function(v) deblack_level=math.floor(v) end})

	SettingsTab:Toggle({Title="Auto Sustain", Desc="Follow MIDI sustain pedal", Value=true,
		Callback=function(v) auto_sustain_enabled=v; refresh_sustain() end})

	SettingsTab:Toggle({Title="Auto Velocity", Desc="Dynamic volume via Sound.Played hooks", Value=true,
		Callback=function(v) auto_velocity_enabled=v; if v then task.spawn(setup_velocity_hooks) end end})

	SettingsTab:Toggle({Title="88 Key Mode", Desc="Full 88-key range (uses Ctrl modifier)", Value=true,
		Callback=function(v) key88_enabled=v end})

	SettingsTab:Toggle({Title="Fast Note-Off", Desc="Release notes early for cleaner sound", Value=false,
		Callback=function(v) FastNoteOff=v; if v then release_all_keys() end end})

	SettingsTab:Slider({Title="Fast Note-Off Delay (s)", Step=0.001,
		Value={Min=0,Max=2,Default=0.02}, Callback=function(v) FastNoteOffDelay=v end})

	SettingsTab:Toggle({Title="Force Note-Off", Desc="Release notes immediately after pressing", Value=false,
		Callback=function(v) no_note_off_enabled=v; if v then release_all_keys() end; refresh_sustain() end})

	SettingsTab:Toggle({Title="Human Player", Desc="Slight random timing for natural feel", Value=false,
		Callback=function(v) random_note_enabled=v end})

	SettingsTab:Slider({
		Title="F11 Start Delay (seconds)",
		Desc="How long to wait after hiding UI before music starts",
		Step=1, Value={Min=0,Max=10,Default=3},
		Callback=function(v) f11_start_delay=math.floor(v) end
	})

	-- ── KEYBOARD LISTENER ────────────────────────────────────
	uis.InputBegan:Connect(function(input, gameProcessed)
		if gameProcessed then return end
		if input.KeyCode == Enum.KeyCode.F11 then f11_play_with_countdown()
		elseif input.KeyCode == Enum.KeyCode.F12 then f12_show_ui() end
	end)

	-- ── RENDER LOOP ──────────────────────────────────────────
	run_service.RenderStepped:Connect(function()
		play_realtime()
		update_timer_and_seek()
		if speed_slider_ref then
			pcall(function()
				ignore_speed = true
				speed_slider_ref:Set(math.floor(playback_speed*100))
				ignore_speed = false
			end)
		end
	end)

	Window:SetToggleKey(Enum.KeyCode.RightControl)
	Window:ToggleTransparency(true)
	Window:SelectTab(1)
	status_label:SetDesc("Ready — Select a song then press Play or F11\nF11 = Hide UI + Play | F12 = Show UI")
end)

run(function()
	if not getgenv().Bypassed then
		getgenv().Bypassed = true
		pcall(function()
			local pkg = Services.ReplicatedStorage:FindFirstChild("Packages")
			if not pkg then return end
			local km = pkg:FindFirstChild("Knit")
			if not km then return end
			local Knit = require(km)
			Knit.OnStart():catch(warn):andThen(function()
				pcall(function()
					local ds = Knit.GetService("DetectionService")
					if ds and ds.WindowFocused then
						while true do ds.WindowFocused:Fire(true); task.wait(1) end
					end
				end)
			end)
			notify("MIDI Player","AntiAfk Bypassed!",3,"bird")
		end)
	end
end)
