local ReplicatedStorage = game:GetService("ReplicatedStorage");
local UserInputService = game:GetService("UserInputService");
local Workspace = game:GetService("Workspace");
local Players = game:GetService("Players");
local LPlayer = Players.LocalPlayer;
local Mouse = LPlayer:GetMouse();
local Remotes: Folder = ReplicatedStorage.Remotes;
local StampAsset: RemoteFunction = Remotes.StampAsset;
local DeleteAsset: RemoteFunction = Remotes.DeleteAsset;
local ActiveParts: Folder;
local Plates: Model = Workspace.Plates;
local LPlate: Part;
local MSpikes = {};
for _, Plate in pairs(Plates:GetChildren()) do
	if (Plate.Owner.Value == LPlayer) then
		LPlate = Plate.Plate;
		ActiveParts = Plate.ActiveParts;
		break;
	end;
end;
local Module = {};
function Module.Kill(Player)
	if (Player:IsA("Player")) then Player = Player.Character.PrimaryPart; end;
	StampAsset:InvokeServer(41324885,LPlate.CFrame - Vector3.new(1e309, 1e309, 1e309),"{99ab22df-ca29-4143-a2fd-0a1b79db78c2}",{Player},0);
end;
local Aura;
function Module.DestroyAura(Radius: number)
	Radius = Vector3.new(Radius, Radius, Radius);
	local Blacklist = {};
	local Hrp = LPlayer.Character.PrimaryPart;
	local Weld = Instance.new("Weld", Hrp);
	Aura = Instance.new("Part", Hrp);
	Aura.Size = Radius;
	Aura.Massless = true;
	Aura.Transparency = 0.7;
	Aura.Material = Enum.Material.Neon;
	Aura.Color = Color3.new(0, 1, 0);
	Aura.CanCollide = false;
	Aura.Shape = Enum.PartType.Ball;
	Aura.Touched:Connect(function(Part)
		local a = game.Players:GetPlayerFromCharacter(Part.Parent)
		if a then Module.Kill(a); end;
	end);
	Weld.Part0 = Hrp;
	Weld.Part1 = Aura;
	Aura.Destroying:Wait();
	table.clear(Blacklist);
	Blacklist = nil;
end;
UserInputService.InputBegan:Connect(function(InputObject, Proccessed)
	if (Proccessed) then return; end;
	if (InputObject.KeyCode == Enum.KeyCode.H) then
		Module.DestroyAura(20);
	elseif (InputObject.KeyCode == Enum.KeyCode.T) then
		Aura:Destroy();
		Aura = nil;
	end;
end);
