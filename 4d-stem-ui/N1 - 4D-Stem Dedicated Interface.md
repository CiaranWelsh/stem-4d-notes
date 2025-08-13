> **Rationale** – A 4D-STEM-first UI minimises cognitive load, keeps routine detector operations safe, and enforces separation of concerns. 
# 1 Capture Context
- **Need ID**: N1
- **Sketch / mock-up**: [Figma (External Link)](https://www.figma.com/board/0Cuwl7vVU8IBp6uOnqVyA6/4d-stem-ui?node-id=1-5&t=pzUdrMNe3fHlafmp-0)
- **External interfaces**: Luna Core, serval rust client and scan box interface (see the [[General Structure Diagram]])
# 2 Four Question Drill Down

## 1 What? 
Provide a dedicated single purpose 4D-STEM user interface separate from the generic detector UI. Generic detector controls are not supported. The most critical user interface controls are in immediate view along with the detector view port. 

## 2 When / Where? 
The user launches an executable on their OS, they are presented with the user interface designed specifically for use in 4D-Stem experiments 

## 3 How well ? 
* Interactive UI, async communication with serval/detector so UI remains responsive (<50us)
* Auto-reconnect on detector disconnection, retry every second. 

## 4 How verified ? 
We need to present the UI to at least 2 domain experts to validate that the workflow works for them on their real system. Feedback generated this way should be used to refine workflows. 

# 3 Functional Requirements

1. The application shall launch in a standalone process when the user double clicks the 4d-stem ui icon or selects it from the native OS menu. 
2. The 4D-stem ui shall not expose unnecessary detector controls to the 4D-stem user. Detector controls are derived from the scan parameters where possible and detector/serval configuration is handled automatically behind the scenes.
	1. <We need a white list of detector controls. What do they actually need>
3. The UI shall present exactly the following scan engine controls to the user. 
		1. Which scan engine is in use. 
		2. Scan geometry parameters, scan height, scan width, <others>? 
		3. Dwell Time
		4. Z - Number of frames (frame = one full scan of HxW)
		5. Scan Start (Search/Preview)
		6. Scan Start (Acquire)
		7. Scan stop (enabled only when running otherwise disabled
4. Sub scanning. The user shall be able to select a region on their sample with a ROI to define the geometry of the scan
5. The user shall be able to select a region on their sample and create a new view port from this selection with the new, zoomed in scope. This new selection is a new preview image containing only the selected pixels. 
6. Users create a workspace for a project. A workspace can contain many measurements. 
7. Measurements contain metadata in json (schema) which are accessible to the user, but initially hidden. 
8. Control settings are dockable widgets that can be hidden or shown via the view menu. 
9. There is a default layout, but users can move menus and controls around. 
10. Persistence - settings and remembered between sessions
11. Connectivity loss shall to be handled gracefully using a 1s retry loop. 
12. No connectivity shall to be handled gracefully 
13. A log of important events shall be written to disk and to console, Level is controlled by an externally available environment variable. Location is a setting under File > preferences but defaults to ~/.accos/accos.log. Serval logs are redirected to ~/.accos/serval.log.