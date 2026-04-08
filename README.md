[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/7awvRogC)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23476315&assignment_repo_type=AssignmentRepo)
# 🧪 Lab 05 – Collectables Upgrade: HUD + Counters + Multi Types

Duration: 2 Hours  
Project Type: Main continuous project (same project from previous labs)

---

## Objective

Upgrade the existing collectables system to support:

- A VR HUD (Head-Up Display)  
- Separate counters for Gems and Keys  
- Multiple collectable types using scripts  

So that when a player collects items, the HUD updates in real time.

---

## Learning Outcomes

By the end of this lab, students will be able to:

- Create a world-space HUD canvas for VR  
- Display real-time counters using TextMeshPro  
- Implement a GameManager using Singleton pattern  
- Create multiple collectable types (Gem and Key)  
- Use enums to classify objects  
- Update UI dynamically from gameplay events  
- Extend an existing collector system  

---

## Software Requirements

- Unity Hub  
- Unity 2022.3 LTS  
- XR Interaction Toolkit  
- OpenXR enabled  
- Windows PC  
- VR Headset (recommended)

---

## Project Name

Use the same project created in previous labs.

Example: VR_PartA_IT21012345


---

## Step-by-Step Instructions

---

## A) Create World Space HUD Canvas

Hierarchy → UI → Canvas  

Rename: **HUD_Canvas**

Inspector → Render Mode = **World Space**

Set Transform: Position: (0, 1.5, 1.2) | Rotation: (0, 180, 0) | Scale: (0.002, 0.002, 0.002


---

## B) Add TMP Texts

Right-click HUD_Canvas → UI → TextMeshPro  

If TMP import popup appears → Import Essentials

Rename:

- TXT_Gems → `Gems: 0`  
- TXT_Keys → `Keys: 0`

---

## C) Create GameManager Object

Hierarchy → Create Empty  

Rename: **GameManager**

---

## D) Script: GameManager.cs

Scripts folder → Create **GameManager.cs**

```csharp
using TMPro;
using UnityEngine;

public class GameManager : MonoBehaviour
{
    public static GameManager Instance { get; private set; }

    public int gems, keys;

    public TMP_Text gemsText;
    public TMP_Text keysText;

    private void Awake()
    {
        if (Instance != null && Instance != this) { Destroy(gameObject); return; }
        Instance = this;
    }

    private void Start() => RefreshUI();

    public void AddGem() { gems++; RefreshUI(); }
    public void AddKey() { keys++; RefreshUI(); }

    public void RefreshUI()
    {
        if (gemsText) gemsText.text = $"Gems: {gems}";
        if (keysText) keysText.text = $"Keys: {keys}";
    }
}
```
---

## E) Attach Script + Assign HUD

Select GameManager

Add Component → GameManager

Assign:

- TXT_Gems → Gems Text  
- TXT_Keys → Keys Text  

---

## F) Script: CollectableItem.cs (Type Info)

Scripts → Create CollectableItem.cs

```csharp
using UnityEngine;

public enum CollectableType { Gem, Key }

public class CollectableItem : MonoBehaviour
{
    public CollectableType type;
}
```

---

## G) Add CollectableItem to Prefabs

Open **Collectable_Gem** prefab

Add Component → CollectableItem  
Set Type = Gem

Create Key prefab:

- Duplicate gem prefab  
- Rename: Collectable_Key  
- Change mesh to Cube  
- Set CollectableItem Type = Key  

---
---

## H) Update CollectorZone.cs

Open CollectorZone.cs and replace code with:

```csharp
using UnityEngine;

public class CollectorZone : MonoBehaviour
{
    public string collectableTag = "Collectable";

    private void OnTriggerEnter(Collider other)
    {
        if (!other.CompareTag(collectableTag)) return;

        var item = other.GetComponent<CollectableItem>();
        if (item != null && GameManager.Instance != null)
        {
            if (item.type == CollectableType.Gem) GameManager.Instance.AddGem();
            if (item.type == CollectableType.Key) GameManager.Instance.AddKey();
        }

        other.gameObject.SetActive(false);
    }
}
```
---

## Testing

1. Press Play  
2. Collect Gems and Keys  
3. Check HUD updates correctly  

---

## Submission Task

Record a video showing:

- Collecting 3 Gems  
- Collecting 1 Key  
- HUD updating correctly  

Filename: Lab05_<YourITNumber>.mp4

Example: Lab05_IT21012345.mp4



---

## Common Problems and Fixes

- HUD not visible → Check Canvas is World Space  
- Text not updating → Check TMP fields assigned in GameManager  
- Counter not increasing → Check CollectableItem component is added  
- Item not collected → Check tag = Collectable  
- HUD facing wrong direction → Adjust canvas rotation  

---


⚠️ **Important Note**

Students must submit **all project files to GitHub** along with the demo video.

Both of the following are mandatory:

- Complete Unity project pushed to a GitHub repository  
- Demo video file uploaded or shared with the submission  

Submissions without the GitHub project files will be considered incomplete.



