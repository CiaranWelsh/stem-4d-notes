We will apply our template to a user need to acquire functional requirements. I will show you the template, the full developing software specification and the give you the requirement that I want you to work on 

The template: 

> **Purpose** – A _minimum routine that converts **one user need** into a fully specified, test‑ready functional requirement (FR) set in ≤ 60 minutes. 

> Copy this note for every need (or run multiple needs in one note—your call).

---

### 1  Capture Context  _(≤ 5 min)_

-  List the **Need ID & summary**.

-  Paste any **sketch / mock‑up / domain diagram** or link to it.

-  Record known **external interfaces / systems** touched by this need.

---

### 2  Run the “4Q Drill‑down”  _(≤ 10 min)_

Write one short bullet per question:

1. **What?** – capability

2. **When/where?** – trigger & scope

3. **How well?** – measurable performance / quality

4. **How verified?** – test or inspection method

---

### 3  Draft Atomic “Shall” Statements  _(≤ 15 min)_

-  Break the answers above into **1‑n FRs**, each **one sentence**.

-  Each FR contains a **trigger/condition** _and_ a **measurable criterion**.

-  If a negative behaviour matters (“shall not …”), capture it too.

---

### 4  Attach Attributes & Test Stub  _(≤ 15 min)_

For _each_ FR add the **inline table** below:

|Field|Value|

|---|---|

|**Need**|N?|

|**Priority**|Must / Should / Could / Won’t|

|**Owner**|🔗 team / person|

|**Assumptions**|HW, lib, env…|

|**Verification**|Test ID + method|

|**Risk**|Low / Med / High|

Then create a **skeleton test** (Gherkin, pytest, etc.) with:

```text

# Requirement: FR‑?.?

# Expectation: <measurable pass/fail>

```

---

### 5  Trace & Review  _(≤ 15 min)_

-  **Link** FR IDs ↔ Need ID in your RTM table or back‑link.

-  **Peer walk‑through** (UX + dev + test) – 10 min time‑box.

-  Mark checklist items DONE ✅, file follow‑ups as tasks.

---

## 📄 Template Block (copy‑paste)

````markdown

### Need N? – <one‑line summary>

![[optional‑sketch.png]]

#### 4Q Drill‑down

- **What?** 

- **When/where?** 

- **How well?** 

- **How verified?** 

#### Functional Requirements

1. **FR‑?.1** The system shall …

2. **FR‑?.2** The system shall …

| Need | Priority | Owner | Assumptions | Verification | Risk |

|------|----------|-------|-------------|--------------|------|

| N? | Must | <name> | <list> | V‑?.1 (automated test) | Med |

```python

# tests/test_fr_?_?.py

def test_fr_xy():

"""Requirement: FR‑?.? – expectation…"""

assert ...

````

---

## 🛠️ Worked Example (condensed)

### Need N3 – Real‑time visual feedback

![[ui‑preview‑sketch.png]]

**4Q**

- **What?** Live preview of scanned region

- **When/where?** Starts when _Arm Scan_ pressed; stops on pause/end

- **How well?** Latency ≤ 50 ms; ≥ 90 % frames at 512×512 / 1 µs dwell

- **How verified?** Loopback latency test @ 1 µs dwell

**FRs**

1. **FR‑3.1** The system shall stream a preview that updates ≤ 50 ms after each detector timestamp when dwell ≥ 1 µs.

2. **FR‑3.2** The preview shall maintain ≥ 90 % frame coverage for a 512×512 probe grid at 1 µs dwell.

3. **FR‑3.3** The preview shall start automatically on _Arm Scan_ and stop on scan end, pause, or abort.

|Need|Priority|Owner|Assumptions|Verification|Risk|

|---|---|---|---|---|---|

|N3|Must|UX Lead|GPU driver supports DMA|V‑3.1 latency CI test|High|

```python

@pytest.mark.parametrize("dwell_ns", [1000])

def test_preview_latency(device, loopback):

"""

Requirement: FR‑3.1 

Expectation: latency ≤ 50 ms

"""

latency = loopback.measure_preview_latency(dwell_ns)

assert latency <= 0.050

```

---

### 📌 Usage Tips

- Keep each checklist run < 1 hour; split big needs.

- Store every note under `SRS/Needs/` to exploit Obsidian backlinks.

- When the FR is implemented, link the **commit hash** or **Jira ticket** here.

---

_End of note_

The Spec: 

\documentclass[18pt]{article}

\usepackage[utf8]{inputenc}

\usepackage{lmodern}

\usepackage{hyperref}

\usepackage{enumitem}

\usepackage{geometry}

\usepackage{longtable}

\usepackage[most]{tcolorbox}

\usepackage{enumitem}

\geometry{margin=1in}

\hypersetup{

colorlinks=true,

linkcolor=blue,

urlcolor=blue,

pdftitle={System Requirements Specification},

pdfpagemode=FullScreen,

}

\usepackage{xcolor} 

% usage: 

% \begin{ReqSection}{FR}

% \item some item

% \end{ReqSection}

% ─────── Generic requirement list with full dotted labels ───────

\newlist{ReqList}{enumerate}{4}

\setlist[ReqList]{leftmargin=*}

% Helper for optional prefix, e.g. "FR", "N", "NFR"

\newenvironment{ReqSection}[1]{%

\def\ReqPrefix{#1}%

\begin{ReqList}[label=\ReqPrefix.\arabic*, ref=\ReqPrefix.\arabic*]}%

{\end{ReqList}}

% Level-2 ⇒ <full L1>.1

\setlist[ReqList,2]{%

label=\theReqListi.\arabic*, % show parent label + dot + own

ref=\theReqListi.\arabic*}

% Level-3 ⇒ <full L2>.1

\setlist[ReqList,3]{%

label=\theReqListii.\arabic*,

ref=\theReqListii.\arabic*}

% Level-4 ⇒ <full L3>.1 (extend similarly for deeper levels)

\setlist[ReqList,4]{%

label=\theReqListiii.\arabic*,

ref=\theReqListiii.\arabic*}

% ---------- MoSCoW tag builder ----------

\newcommand{\PriorityTag}[2]{% #1 = text, #2 = base colour

\colorbox{#2!25}{\footnotesize\textsf{\textbf{#1}}}\hspace{0.6em}}

\newcommand{\must}{\leavevmode\PriorityTag{Must}{green}}

\newcommand{\should}{\leavevmode\PriorityTag{Should}{yellow}}

\newcommand{\could}{\leavevmode\PriorityTag{Could}{cyan}}

\newcommand{\wont}{\leavevmode\PriorityTag{Won't}{red}}

\title{4D-STEM Tool Requirements Specification}

\author{Ciaran Welsh / SEM Team}

\date{\today}

\begin{document}

\maketitle

\tableofcontents

\newpage

% FYI: Template I'm following can be found here: https://www.perforce.com/blog/alm/how-write-software-requirements-specification-srs-document

\section{Introduction}

Timepix3‑based detectors have opened the door to 4D‑STEM <with these advantages>. Today, Serval and its client, Accos, handle both generic detector acquisition and a bolted‑on 4D‑STEM workflow, forcing two different usage patterns into one platform. This constrains usability for 4D-STEM users as they need to activate the hidden corner of the Accos before they can use 4D-STEM tools. This also constrains stability for the regular detector control user because additions needed for the newer 4D-STEM can touch other parts of Accos.

This document proposes a clear separation: a purpose‑built 4D‑STEM acquisition stack that reuses Serval, Luna and components of accos, but not accos. 

This project delivers a dedicated 4D‑STEM acquisition application that streamlines this capability for microscopists, decoupling it from our general detector‑control software so both user communities benefit from focused, optimized workflows.

* performance of event based compared to frame based. 

* Increased storage efficiency. 

\subsection{Purpose}

% Start by clearly defining why the software is being built and what it aims to achieve. This introduction sets expectations that you’ll revisit throughout the SRS. 

Our goal is to build a new software system for use specifically in 4D-STEM applications. Microscope operatives will use this tool as their primary driver for the 4D-stem solution offered by ASI. 

Our existing tool, Accos, has been overloaded by the addition of 4D-stem features and it doesn't make sense for either the 4D-Stem users nor the regular "Detector Control" users. Excising the tool into a 

This system must leverage existing work 

% Some items to keep in mind when defining this purpose include:

\subsubsection{Intended Audience and Use}

% Define who in your organization will have access to the SRS and how they should use it. This may include developers, testers, and project managers. It could also include stakeholders in other departments, including leadership teams, sales, and marketing. Defining this now will lead to less work in the future.

This document should be used by everybody involved in the development process. Application scientists define the feature list and developers use the list to understand what to build. The sales team can use this document as intel in what to expect in future. 

\subsubsection{Product Scope}

% Explain the broader objectives and benefits of the software. How does it align with your organization’s goals?

The boundary of what we implement is LiberTEM and HyperSpy. I.e. inpu = stream of events from detector. Output is vADF or image in real space, and the diffraction patterns at each probe position. 

\subsubsection{Definitions and Acronyms}

% Clearly define all key terms, acronyms, and abbreviations used in the SRS. This will help eliminate any ambiguity and ensure that all parties easily understand the document.

\begin{itemize}

\item CBED

\item vADF

\item iDPC

\item iCOM

\item 

\end{itemize}

\section{Overall Description}

\subsection{User Needs}

% “User needs” capture the *problem space* (what the people using the product are trying to accomplish) while “Features” capture the *solution space* (what the system will do to meet those needs).} Putting them in separate sections makes it possible to trace every feature back to a genuine user demand and to challenge any feature that has no such lineage.

\begin{ReqSection}{N}

\item \textbf{Dedicated 4D-STEM interface separate from the generic detector UI}\\

\textit{Rationale: Build with a 4D-stem first mentality. Makes software 4d-stem centric, minimises cognitive load on our users needing to compete with more general use cases and prevents accidental changes to routine detector operations. Separation of concerns.}

\item \textbf{Immediate, clearly organized access to all scan-generator controls}\\

\textit{Rationale: Reduces set-up time, enhances usability and ensures detector settings stay consistent with scan parameters.}

\item \textbf{Seamless integration with Serval for detector control whilst being exposed to only what they need}\\

\textit{Rationale: Serval must be used for detector control but minimizing the user interaction with Serval reduces the learning curve and enhances usability}

\item \textbf{Real-time visual feedback during scans}\\

\textit{Rationale: Enables rapid navigation to regions of interest and immediate quality assessment. Also prevents wasted beam time.}

\item \textbf{Acquire continuous or multi-frame (“Z-stack”) scans}\\

\textit{Rationale: Lets users monitor radiation damage frame-by-frame.}

\item \textbf{Ability to run essential offline analyses inside the application}\\

\textit{Rationale: Enables quality assessment decisions without exporting large datasets to external tools.}

\item \textbf{Flexible detector masking (hardware or software) to include/exclude pixels before acquisition.}\\

\textit{Rationale: Controls data volume and tailor field-of-view after initial acquisition in post-processing.}

\item \textbf{Interactive pixel group selection for averaged diffraction images}\\

\textit{Rationale: Users can observe averaged regions of sample in diffraction space.}

\item \textbf{Switchable scan modes to balance performance and user feedback trade-offs}\\

\textit{Rationale: When searching for interesting features for capture, we need to optimize performance, and low latency preview. When acquiring data "for real", we optimize data integrity for further analysis.}

\item \textbf{Export raw \emph{or} processed data in open formats (e.g.\ SparseArray, HDF5, Zarr) and where appropriate, image data}\\

\textit{Rationale: Guarantees images can be saved and interoperability with analysis suites such as LiberTEM, HyperSpy, and py4DSTEM.}

\item \textbf{Real-time observability of acquisition-pipeline health (hit-rate, processing load, synchronisation)}\\

\textit{Rationale: Gives user feedback and "sanity checkibility" for operatives, indicates dropped frames and that hit rate is expected.}

\item \textbf{Data integrity/fidelity at extreme rates}\\

\textit{Rationale: Ensures every diffraction event is mapped to the right probe position even in the fastest scans, preserving scientific validity.}

\item \textbf{Extensibility hooks for future 4D-STEM modalities behind feature flags}\\

\textit{Rationale: Makes the tool relevant for future and lets advanced users adopt new workflows (e.g.\ EELS, ptychography) without redesigning the core tool.}

\item \textbf{Possibility to use this tool with Tpx4}\\

\textit{Rationale: We build for the future now by supporting the next generation Timepix technology from initial design.}

\end{ReqSection}

\subsection{Assumptions and Dependencies}

\begin{itemize}

\item Serval is required for detector control

\item Currently serval is also required for setting up the raw TCP stream

\item Luna is required for heavy data processing

\item A UI library, probably flutter with its "bridge" to rust.

\end{itemize}

\section{System Features and Requirements}

\subsection{Functional Requirements}

\subsubsection{UI Shell}

The following list is a set of features that we expect to see in the GUI

\begin{ReqSection}{FR1}

\item \must The application shall launch in a standalone process when the user double-clicks the 4D-STEM UI icon or selects it from the native OS menu.

\item The 4D-STEM UI shall not expose unnecessary detector controls to the 4D-STEM user. Detector controls are derived from the scan parameters where possible, and detector/Serval configuration is handled automatically behind the scenes.

\begin{ReqList}

\item \textit{[Note: We need a whitelist of detector controls. What do they actually need, bias?]}

\end{ReqList}

\item The UI shall present exactly the following scan-engine controls to the user. Scan engine controls are immediately and always visible and accessible.

\begin{ReqList}

\item Which scan engine is in use.

\item Scan geometry parameters: scan height, scan width, \textit{[others?]}

\item Dwell time.

\item Z – Number of frames (frame = one full scan of H$\times$W).

\item Scan Start (Search/Preview).

\item Scan Start (Acquire).

\item Scan Stop (enabled only when running; otherwise disabled).

\end{ReqList}

\item Sub-scanning: the user shall be able to select a region on their sample with an ROI to define the geometry of the scan.

\item The user shall be able to select a region (ROI) on their sample with UI click and drag.

\begin{ReqList}

\item ROIs shall be translated into scan geometries.

\item ROIs shall be expandable into a full larger resolution image containing the selected pixels.

\item ROIs shall be used to build diffraction images.

\end{ReqList}

\item Users create a workspace for a project. A workspace can contain many measurements.

\item Measurements contain metadata in JSON (schema) which are accessible to the user but initially hidden.

\item Control settings are dockable widgets that can be hidden or shown via the view menu.

\item There is a default layout, but users can move menus and controls around.

\item Persistence: settings are remembered between sessions.

\item Connectivity loss shall be handled gracefully using a 1 s retry loop.

\item No connectivity at startup shall be handled gracefully.

\item A log of important events shall be written to disk and to console. The log level is controlled by an externally available environment variable. The log location is a setting under \texttt{File > Preferences} but defaults to \texttt{\~/.accos/accos.log}. Serval logs are redirected to \texttt{\~/.accos/serval.log}.

\end{ReqSection}

\begin{ReqSection}{FR2}

\item 

\end{ReqSection}

\subsubsection{Numbered and Described}

\subsubsection{EARS Format}

% Example: When [event], the system shall [response].

\subsubsection{Specification by Example / BDD Format}

% Use Gherkin syntax:

% Given [context], When [event], Then [outcome]

\subsection{Non-Functional Requirements}

\subsubsection{Performance}

% e.g., "95\% of requests shall return in under 2 seconds"

\subsubsection{Security}

% e.g., "Only authenticated users can access admin API"

\subsubsection{Usability, Reliability, Compliance}

Hardware Compatibility

It should not be difficult to support tpx4, when the time comes to upgrade.

% \subsection{External Interface Requirements}

% \subsubsection{Performance Requirements}

% \subsubsection{Safety Requirements}

% \subsubsection{Security Requirements}

% \subsubsection{Software Quality Attributes}

% \subsubsection{Business Rules}

\subsection{System Features}

% Optional: Detailed listing of major features if not captured above.

% \section{Other Requirements}

% \subsection{Database Requirements}

\subsection{Legal and Regulatory Requirements}

Licenses. What can we do? What can't we do? What software license do we provide?

% \subsection{Internationalization and Localization}

% \subsection{Risk Management (FMEA Matrix)}

% % Optionally use a longtable or tabular environment for the matrix

\section{Appendices}

\subsection{Glossary}

\subsection{Use Cases and Diagrams}

\begin{figure}

\centering

\includegraphics[width=0.75\linewidth]{GeneralStructure.png}

\caption{Broad overview of the tools and repositories needed}

\label{fig:structural_overview}

\end{figure}

% Include UML, flowcharts, sequence diagrams, etc.

\subsection{To Be Determined (TBD) List}

% Track any unresolved items

\end{document}

% Talk with Fran

% \begin{itemize}

% \item Scan parameters and view port/window are prominent in first window

% \begin{itemize}

% \item Search, preview, acquire are all different modes of acquisition. 

% \end{itemize}

% \item vADF: By default, the image reconstruction method will be vADF. 

% \item iDPC: Better contrast. Better does efficient compared to vADF. 

% \item iCOM: offline analysis. Average CBED of the full frame is needed before iCOM can be computed.

% \item Software masking, but we also need hardware masking. 

% \begin{itemize}

% \item If hardware and then you want the full CBED, then you need to reacquire -> doubl edose for same reigon. 

% \item If software, then you do post-processing and you have the data there. 

% \end{itemize}

% \item Synchronisation between scan gen and detector start 

% \item Smaller window or popup where you can control the software mask whilst collecting data. No modify mask whilst acquiring. If you change mask while scanning, no update. 

% \item Output

% \begin{itemize}

% \item Export Tiff files for any and all images, this needs to be an option. 

% \begin{itemize}

% \item Sometimes users want to export as tiff/hdf5/sparse but only selections thereof. 

% \end{itemize}

% \item Raw output data

% \item We should be able to build a "frame". Every scan positon has a CBED associated. We should be able to build a Tiff image of the CBED. Does not need to be online.

% \begin{itemize}

% \item Selection of ROI's and perofrm computation (Averages mostly) of this subset of data. (Cherry on top)

% \item Output as

% \end{itemize}

% \end{itemize}

% \item Throughout the scan, each scan position has a difftaction pattern and this

% \item Sorting means assignment of events to scan position in real space (does not require absolute sorting), though sparse output

% \item If 1us dwell time is slowest that we sort, then how much speed do we need to sort with? 

% \begin{itemize}

% \item If x is our dwell time then what performance requirement do we need in sorting? 

% \end{itemize}

% \begin{itemize}

% \item Performance is a huge part of what we do here. Right now we are aiming for 320Mhits / s for felis T3

% \end{itemize}

% \item Real vs diffraction space

% \begin{itemize}

% \item Real, is the inverse foriour transform of the diffraction space. Users will want to export real space as images. This is intensities. Basically this is a matrix with number, bottom line. Same with diffraction. 

% \item Diffraction space - same thing. But here user would want to export both image data and events as hdf5/ZARR/Sparse data too.

% \end{itemize}

% \item Continuous scans. Users would like to take multiple frames. This is a z stack. 512x512xnumber of scans. For instance, 10 CBEDs for each probe position when "Z" is 10. 

% \begin{itemize}

% \item Ideally you wouldkeep z frames separate because then you can monitor degredation of you same by eye. But it consumes a lot of space.

% \item Degradation is a measurable quantity, but might not be worth it. Different samples require different damage assessment strategies. This feature requires building lots of them and relying on them to know which to use. So maybe not... possibly a hook for them. 

% \item Variance of real space image. Resolution of image is linked to the variance of its grey scale intensities. 

% \begin{itemize}

% \item Rabbit hole.

% \end{itemize}

% \end{itemize}

% \begin{itemize}

% \item Space concerns. 

% \end{itemize}

% \item Sparse Arrays as output

% \end{itemize}

% Future outlook

% \begin{itemize}

% \item 4Dstem EELS, building 1D spectra. Possibly hidden behind feature flag. 

% \end{itemize}

And I want you to work on this specific "Need"