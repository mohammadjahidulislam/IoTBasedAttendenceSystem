Name:Md Jahidul Islam

ID:It24018

# IoTBasedAttendenceSystem


Latex code for project proposal:






\documentclass[11pt,a4paper]{article}
 
\usepackage[margin=1in]{geometry}

\usepackage[T1]{fontenc}

\usepackage{graphicx}

\usepackage{booktabs}

\usepackage{array}

\usepackage{longtable}

\usepackage{float}

\usepackage{enumitem}
\usepackage{xcolor}
\usepackage{listings}
\usepackage{pgfgantt}
\usepackage{tikz}
\usetikzlibrary{shapes.geometric,shapes.misc,shapes.multipart,arrows.meta,positioning,fit,calc,backgrounds}
\usepackage[colorlinks=true,linkcolor=blue!50!black,urlcolor=blue!60!black,citecolor=blue!50!black]{hyperref}
 
\lstdefinestyle{java}{
  language=Java,
  basicstyle=\ttfamily\footnotesize,
  keywordstyle=\color{blue!60!black}\bfseries,
  commentstyle=\color{green!40!black}\itshape,
  stringstyle=\color{red!60!black},
  numbers=left, numberstyle=\tiny\color{gray},
  breaklines=true, frame=single, framerule=0.3pt,
  backgroundcolor=\color{gray!6},
  showstringspaces=false, tabsize=2
}
 
\newcommand{\projtitle}{IoT Based Smart Attendance System }
 
\title{\projtitle}
\author{Md Jahidul Islam}
\date{\today}
 
\begin{document}
 
%=====================================================================
% TITLE PAGE
%=====================================================================
\begin{titlepage}
\centering
\vspace*{0.8cm}
{\LARGE\bfseries \projtitle\par}
\vspace{0.3cm}
{\large with ESP32, RFID/QR Scanning and a Java Cloud Dashboard\par}
\vspace{0.3cm}
{\normalsize Final Capstone Project -- Part I: Proposal and Prototyping\par}
\vspace{1.4cm}
 
{\renewcommand{\arraystretch}{1.7}
\begin{tabular}{|>{\bfseries}p{4cm}|p{8.5cm}|}
\hline
\multicolumn{2}{|c|}{\textbf{Submitted By (Student)}}\\
\hline
Name & Md Jahidul Islam\\
\hline
Student ID & IT24018\\
\hline
Course & Software Engineering and Project Management\\
\hline
Department & Information and Communication Technology\\
\hline
University & Mawlana Bhashani Science and Technology University\\
\hline
\end{tabular}}
 
\vspace{1.0cm}
 
{\renewcommand{\arraystretch}{1.7}
\begin{tabular}{|>{\bfseries}p{4cm}|p{8.5cm}|}
\hline
\multicolumn{2}{|c|}{\textbf{Supervised By (Teacher)}}\\
\hline
Name & Dr. Ziaur Rahman\\
\hline
Designation & Professor\\
\hline
Department & Dept. of ICT\\
\hline
University & MBSTU\\
\hline
\end{tabular}}
 
\vspace{1.0cm}
\textbf{Date:} \today\\[6pt]
\textbf{GitHub:} \url{https://github.com/mohammadjahidulislam/IoTBasedAttendenceSystem}
\vfill
\end{titlepage}
 
\begin{abstract}
Manual roll-call wastes class time and is prone to proxy attendance and recording errors. This proposal describes an IoT-based attendance system in which a classroom device built around an ESP32 microcontroller reads RFID cards (and QR codes) of students, validates the scan, and sends it over Wi-Fi using MQTT/HTTPS to a Java server. Teachers see live attendance on a dashboard and obtain reports automatically. The document presents the problem, objectives, requirements, hardware and software architecture, an Iterative-Incremental development plan with schedule, milestones, deliverables and Gantt chart, the six required UML diagrams, a risk and cost analysis, and a simple Java prototype with more than five classes taken from the class diagram.
\end{abstract}
 
\newpage
\tableofcontents
\newpage
 
%=====================================================================
\section{Introduction}
\subsection{Background}
Attendance is a basic academic requirement, yet most classes still use paper sheets or verbal roll-call. This takes several minutes per class, and signatures can be forged. Internet of Things (IoT) devices are cheap, small and always connected, so they are a natural fit for automating identification and reporting.
 
\subsection{Problem Statement}
\begin{itemize}[noitemsep]
  \item Manual attendance consumes lecture time and is error-prone.
  \item Proxy attendance (one student signing for another) is hard to detect.
  \item Consolidating paper sheets into semester reports is slow.
  \item Teachers get no real-time view of who is present.
\end{itemize}
 
\subsection{Objectives}
\begin{enumerate}[noitemsep]
  \item Design and build an ESP32-based classroom device that identifies students by RFID card or QR code.
  \item Transmit each scan securely to a central server in near real time.
  \item Store attendance in a relational database and prevent duplicate or invalid entries.
  \item Provide a teacher dashboard with live status, history and CSV/PDF reports.
  \item Keep working during short network outages by buffering scans on the device.
  \item Deliver the work using a documented software process with schedule, milestones and UML models.
\end{enumerate}
 
\subsection{Scope}
\textbf{In scope:} one device per classroom, student identification, live dashboard, reports, device management, offline buffering.
\textbf{Out of scope (this phase):} face recognition, payment or library integration, native mobile apps, integration with the university ERP.
 
\subsection{Significance}
The system saves lecture time, improves data accuracy, discourages proxy attendance and provides a practical, low-cost IoT project that can be scaled to many rooms.
 
%=====================================================================
\section{Review of Existing Approaches}
\begin{table}[H]
\centering
\small
\begin{tabular}{p{3.2cm}p{4.2cm}p{4.8cm}}
\toprule
\textbf{Approach} & \textbf{Advantage} & \textbf{Limitation}\\
\midrule
Paper roll-call & No cost & Slow, easy to forge, hard to analyse\\
Biometric (fingerprint) & Hard to fake & Costly, queues, hygiene concerns\\
App-only QR attendance & No extra hardware & Phone sharing, GPS spoofing, needs student phones\\
\textbf{Proposed: ESP32 + RFID/QR} & Low cost, fast tap, physical presence required, real-time data & Needs Wi-Fi and cards; mitigated by offline buffer\\
\bottomrule
\end{tabular}
\caption{Comparison of attendance approaches.}
\end{table}
 
%=====================================================================
\section{Proposed System}
\subsection{Overview}
A student taps an RFID card (or shows a QR code) at the classroom device. The ESP32 reads the unique ID, stamps it with time from the real-time clock, and publishes it to the server. The server validates the student against the active class session, stores the record, and sends back a result that the device shows on an OLED screen with a buzzer/LED signal. The teacher watches the dashboard update live.
 
\subsection{System Architecture}
\begin{figure}[H]
\centering
\begin{tikzpicture}[
  node distance=0.9cm and 0.9cm,
  box/.style={draw,rounded corners,align=center,minimum height=1.0cm,minimum width=2.3cm,font=\small,fill=blue!6},
  dev/.style={draw,rounded corners,align=center,minimum height=1.0cm,minimum width=2.3cm,font=\small,fill=orange!12},
  srv/.style={draw,rounded corners,align=center,minimum height=1.0cm,minimum width=2.3cm,font=\small,fill=green!10},
  arr/.style={-{Stealth[length=2.5mm]},thick}
]
\node[dev] (card) {Student\\RFID card / QR};
\node[dev,right=of card] (esp) {ESP32 device\\(RC522, OLED,\\buzzer, RTC)};
\node[box,right=of esp] (mqtt) {Wi-Fi\\MQTT / HTTPS};
\node[srv,right=of mqtt] (srv) {Java server\\(Javalin API)};
\node[srv,below=1.1cm of srv] (db) {Database\\(MySQL)};
\node[box,below=1.1cm of mqtt] (dash) {Teacher\\dashboard};
\node[box,below=1.1cm of esp] (admin) {Admin\\panel};
\draw[arr] (card) -- node[above,font=\scriptsize]{tap} (esp);
\draw[arr] (esp) -- node[above,font=\scriptsize]{scan} (mqtt);
\draw[arr] (mqtt) -- (srv);
\draw[arr] (srv) -- (db);
\draw[arr] (srv) -- (dash);
\draw[arr] (admin) -- (srv);
\draw[arr,dashed] (srv.north) -- ++(0,0.7) -| node[pos=0.25,above,font=\scriptsize]{result} (esp.north);
\end{tikzpicture}
\caption{High-level IoT architecture.}
\end{figure}
 
\subsection{Working Flow}
\begin{enumerate}[noitemsep]
  \item The teacher starts a class session on the dashboard.
  \item The student taps a card; the ESP32 reads the UID.
  \item The ESP32 publishes \texttt{\{uid, deviceId, timestamp\}} to topic \texttt{attendance/scan}.
  \item The server checks that the UID belongs to a student enrolled in the active course and has not already been marked.
  \item An attendance record (Present or Late) is saved and the result is returned to the device.
  \item The OLED shows the student's name; green LED and one beep mean success, red LED and two beeps mean rejection.
  \item If Wi-Fi is down, the ESP32 stores scans in flash memory and uploads them when the connection returns.
\end{enumerate}
 
%=====================================================================
\section{Hardware Components}
The component details below were collected from manufacturer and retailer product pages for the ESP32 DevKit V1 and the RC522 RFID module. Prices vary by seller and country, so the cost column is only a planning estimate that must be re-checked before purchase.
 
\begin{table}[H]
\centering
\small
\begin{tabular}{p{2.9cm}p{6.4cm}p{1.6cm}p{1.5cm}}
\toprule
\textbf{Component} & \textbf{Key specifications} & \textbf{Role} & \textbf{Est. cost (BDT)}\\
\midrule
ESP32 DevKit V1 (ESP-WROOM-32) & Dual-core Xtensa LX6 up to 240\,MHz, 520\,KB SRAM, 4\,MB flash, Wi-Fi 802.11 b/g/n, Bluetooth 4.2/BLE, about 34 GPIO, 3.3\,V logic & Main controller & 600--900\\
RC522 RFID module (MFRC522) & 13.56\,MHz, SPI up to 10\,Mbit/s, 3.3\,V only, about 13--26\,mA when active, reads ISO/IEC 14443-A / MIFARE cards at roughly 2--5\,cm, usually sold with one card and one key tag & Student identification & 150--300\\
RFID cards / key tags (MIFARE 1K) & 13.56\,MHz passive tags, unique UID per tag & Student ID & 20--40 each\\
0.96'' OLED (SSD1306) & I2C, 128$\times$64 pixels & Status and name display & 250--400\\
DS3231 RTC module & I2C, battery-backed accurate clock & Timestamps when offline & 250--400\\
Buzzer and LEDs (red/green) & Active buzzer, 5\,mm LEDs, resistors & Audio and visual feedback & 50--100\\
5\,V USB power supply & 5\,V, 1\,A or more & Power & 150--300\\
Optional GM65 QR scanner & UART barcode/QR reader module & QR code input & 1200--1800\\
\bottomrule
\end{tabular}
\caption{Bill of materials with specifications (costs are estimates).}
\end{table}
 
\subsection{Wiring Plan (ESP32 to Modules)}
\begin{table}[H]
\centering
\small
\begin{tabular}{lll}
\toprule
\textbf{Module pin} & \textbf{ESP32 pin} & \textbf{Note}\\
\midrule
RC522 SDA (SS) & GPIO 5 & SPI chip select\\
RC522 SCK & GPIO 18 & SPI clock\\
RC522 MOSI & GPIO 23 & SPI data out\\
RC522 MISO & GPIO 19 & SPI data in\\
RC522 RST & GPIO 27 & Reset\\
RC522 3.3V / GND & 3V3 / GND & Never connect to 5\,V\\
OLED and RTC SDA / SCL & GPIO 21 / GPIO 22 & Shared I2C bus\\
Green LED / Red LED & GPIO 25 / GPIO 26 & With 220\,$\Omega$ resistor\\
Buzzer & GPIO 33 & Active buzzer\\
\bottomrule
\end{tabular}
\caption{Proposed pin mapping.}
\end{table}
 
\subsection{Device Firmware Outline}
The firmware (Arduino C++ with the \texttt{MFRC522}, \texttt{PubSubClient} and \texttt{Adafruit\_SSD1306} libraries) runs a loop: connect Wi-Fi $\rightarrow$ connect MQTT $\rightarrow$ poll RC522 $\rightarrow$ build JSON $\rightarrow$ publish $\rightarrow$ wait for result topic $\rightarrow$ show feedback. Unsent scans are saved to a small queue in flash.
 
%=====================================================================
\section{Requirements}
\subsection{Functional Requirements}
\begin{longtable}{p{1.2cm}p{13cm}}
\toprule
\textbf{ID} & \textbf{Requirement}\\
\midrule
\endhead
FR1 & The device shall read the UID of an RFID card and/or decode a QR code.\\
FR2 & The device shall send each scan with device ID and timestamp to the server.\\
FR3 & The server shall validate that the student is enrolled in the active course.\\
FR4 & The server shall reject duplicate scans within the same session.\\
FR5 & The system shall mark a student Present or Late according to a configurable threshold.\\
FR6 & The device shall give visual and audio feedback within 2 seconds.\\
FR7 & The device shall buffer scans while offline and synchronise later.\\
FR8 & The teacher shall start and end class sessions and view live attendance.\\
FR9 & The admin shall register students, cards, courses and devices.\\
FR10 & The system shall generate attendance reports per student, course and date, exportable as CSV.\\
\bottomrule
\end{longtable}
 
\subsection{Non-Functional Requirements}
\begin{itemize}[noitemsep]
  \item \textbf{Performance:} end-to-end scan to feedback in under 2\,s on a normal Wi-Fi network.
  \item \textbf{Security:} TLS for network traffic, hashed passwords, role-based access, per-device credentials.
  \item \textbf{Reliability:} no scan loss during outages up to several hours.
  \item \textbf{Usability:} one tap, no training required for students.
  \item \textbf{Maintainability:} layered Java code, documented API, version control on GitHub.
  \item \textbf{Scalability:} support at least 20 devices on one server.
\end{itemize}
 
\subsection{Technology Stack}
\begin{table}[H]
\centering
\small
\begin{tabular}{ll}
\toprule
\textbf{Layer} & \textbf{Technology}\\
\midrule
Device firmware & Arduino C++ on ESP32\\
Messaging & MQTT (Mosquitto broker) or HTTPS REST\\
Server & Java 17, Javalin, JDBC\\
Database & MySQL / PostgreSQL\\
Dashboard & HTML/JavaScript served by Javalin; JavaFX for the prototype\\
Documentation & \LaTeX, TikZ, pgfgantt\\
Version control & Git and GitHub\\
\bottomrule
\end{tabular}
\caption{Technology stack.}
\end{table}
 
%=====================================================================
\section{Software Process Model and Project Plan}
\subsection{Chosen Model: Iterative and Incremental}
The Iterative-Incremental model is chosen because (1) hardware and software can be delivered in small working increments, (2) early prototypes with a simulated device reduce risk while parts are being sourced, (3) feedback after each increment can correct requirements, and (4) a working system exists after the first increments even if time runs short.
 
\begin{table}[H]
\centering
\small
\begin{tabular}{llp{8cm}}
\toprule
\textbf{Increment} & \textbf{Weeks} & \textbf{Content}\\
\midrule
I1 & 3--6 & Core server, database, class model, device simulator, basic JavaFX interface\\
I2 & 7--9 & Real ESP32 + RC522 firmware, MQTT integration, OLED/buzzer feedback\\
I3 & 10--12 & Dashboard, reports, offline buffering, security\\
I4 & 13--14 & Full testing, documentation, demo and final delivery\\
\bottomrule
\end{tabular}
\caption{Planned increments.}
\end{table}
 
\subsection{Work Breakdown and Schedule}
\begin{table}[H]
\centering
\small
\begin{tabular}{clcc}
\toprule
\textbf{No.} & \textbf{Task} & \textbf{Start (week)} & \textbf{End (week)}\\
\midrule
1 & Proposal, requirements and UML design & 1 & 2\\
2 & Component procurement and setup & 2 & 4\\
3 & Database and server core (I1) & 3 & 5\\
4 & Java prototype and device simulator (I1) & 4 & 6\\
5 & ESP32 firmware and RFID reading (I2) & 6 & 8\\
6 & MQTT/API integration (I2) & 8 & 9\\
7 & Teacher dashboard and reports (I3) & 9 & 11\\
8 & Offline buffering and security (I3) & 11 & 12\\
9 & System testing and bug fixing (I4) & 12 & 13\\
10 & Final report, demo and release (I4) & 13 & 14\\
\bottomrule
\end{tabular}
\caption{Project schedule.}
\end{table}
 
\subsection{Gantt Chart}
\begin{figure}[H]
\centering
\resizebox{\textwidth}{!}{%
\begin{ganttchart}[
  hgrid, vgrid={*{14}{dotted}},
  x unit=0.6cm, y unit chart=0.62cm,
  title height=1, title label font=\scriptsize\bfseries,
  bar label font=\scriptsize, bar height=0.5,
  group label font=\scriptsize\bfseries,
  bar/.append style={fill=blue!45},
  milestone/.append style={fill=red!70, xscale=1.0}
]{1}{14}
\gantttitle{Weeks}{14}\\
\gantttitlelist{1,...,14}{1}\\
\ganttbar{Proposal \& UML design}{1}{2}\\
\ganttbar{Procure components}{2}{4}\\
\ganttbar{Database \& server core}{3}{5}\\
\ganttbar{Java prototype \& simulator}{4}{6}\\
\ganttbar{ESP32 firmware + RFID}{6}{8}\\
\ganttbar{MQTT/API integration}{8}{9}\\
\ganttbar{Dashboard \& reports}{9}{11}\\
\ganttbar{Offline buffer \& security}{11}{12}\\
\ganttbar{Testing \& bug fixing}{12}{13}\\
\ganttbar{Final report \& demo}{13}{14}\\
\ganttmilestone{M1 Proposal approved}{2}\\
\ganttmilestone{M2 Java prototype}{6}\\
\ganttmilestone{M3 ESP32 working}{9}\\
\ganttmilestone{M4 Feature-complete}{12}\\
\ganttmilestone{M5 Final delivery}{14}
\end{ganttchart}}
\caption{Gantt chart of the 14-week plan.}
\end{figure}
 
\subsection{Milestones}
\begin{table}[H]
\centering
\small
\begin{tabular}{llp{7.5cm}}
\toprule
\textbf{ID} & \textbf{Week} & \textbf{Milestone and exit criteria}\\
\midrule
M1 & 2 & Proposal approved: requirements and all UML diagrams complete\\
M2 & 6 & Java prototype runs: simulated scan creates an attendance record\\
M3 & 9 & Real ESP32 device sends an RFID scan to the server\\
M4 & 12 & Feature-complete: dashboard, reports, offline sync, security\\
M5 & 14 & Final delivery: tested system, report, demo and GitHub release\\
\bottomrule
\end{tabular}
\caption{Project milestones.}
\end{table}
 
\subsection{Deliverables}
\begin{table}[H]
\centering
\small
\begin{tabular}{llp{6.5cm}}
\toprule
\textbf{No.} & \textbf{Deliverable} & \textbf{Description}\\
\midrule
D1 & Project proposal & This document with UML models (week 2)\\
D2 & Java prototype & JavaFX interface with 5+ classes and device simulator (week 6)\\
D3 & Device prototype & Working ESP32 + RC522 unit with wiring diagram (week 9)\\
D4 & Server and dashboard & Source code, API documentation, database script (week 12)\\
D5 & Test report & Test cases and results (week 13)\\
D6 & Final report and demo & Report, slides and demo video (week 14)\\
D7 & Repository & Tagged release on GitHub with README (week 14)\\
\bottomrule
\end{tabular}
\caption{Project deliverables.}
\end{table}
 
%=====================================================================
\section{Risk Analysis}
\begin{table}[H]
\centering
\small
\begin{tabular}{p{3.4cm}cp{3.2cm}p{4.5cm}}
\toprule
\textbf{Risk} & \textbf{Level} & \textbf{Impact} & \textbf{Mitigation}\\
\midrule
Delay in component delivery & High & Hardware milestone slips & Build the Java simulator first; order parts early\\
Wi-Fi outage in classroom & Medium & Lost scans & Offline buffer in device flash\\
Card sharing or cloning & Medium & Proxy attendance & Optional PIN/QR second factor; log device and time\\
Module damage (5\,V on 3.3\,V pins) & Medium & Replacement cost & Wiring checklist; buy a spare RC522\\
Scope creep & Medium & Schedule overrun & Fixed scope list; incremental delivery\\
Data privacy & Medium & Student data exposure & TLS, access control, minimal data stored\\
\bottomrule
\end{tabular}
\caption{Risk register.}
\end{table}
 
%=====================================================================
\section{UML Diagrams}
 
\subsection{Data Flow Diagram}
\subsubsection*{Level 0 (Context Diagram)}
\begin{figure}[H]
\centering
\begin{tikzpicture}[
  ent/.style={draw,rectangle,minimum width=2.2cm,minimum height=0.9cm,align=center,font=\small,fill=yellow!15},
  proc/.style={draw,circle,minimum size=2.6cm,align=center,font=\small,fill=blue!8},
  arr/.style={-{Stealth[length=2.3mm]},thick},
  lab/.style={font=\scriptsize,fill=white,inner sep=1pt}
]
\node[proc] (p) at (0,0) {0\\IoT Attendance\\System};
\node[ent] (stu) at (-5,1.8) {Student};
\node[ent] (tea) at (5,1.8) {Teacher};
\node[ent] (adm) at (-5,-1.8) {Admin};
\node[ent] (dev) at (5,-1.8) {ESP32 Device};
\draw[arr] (stu) -- node[lab,pos=0.45]{RFID/QR tap} (p);
\draw[arr] (p) -- node[lab,pos=0.55]{live status, reports} (tea);
\draw[arr] (tea) to[bend left=18] node[lab,pos=0.5]{start/end session} (p);
\draw[arr] (adm) -- node[lab,pos=0.45]{register data} (p);
\draw[arr] (dev) to[bend left=18] node[lab,pos=0.5]{scan events} (p);
\draw[arr] (p) to[bend left=18] node[lab,pos=0.5]{result} (dev);
\end{tikzpicture}
\caption{DFD Level 0.}
\end{figure}
 
\subsubsection*{Level 1}
\begin{figure}[H]
\centering
\resizebox{\textwidth}{!}{%
\begin{tikzpicture}[
  ent/.style={draw,rectangle,minimum width=2cm,minimum height=0.8cm,align=center,font=\small,fill=yellow!15},
  proc/.style={draw,circle,minimum size=2.1cm,align=center,font=\scriptsize,fill=blue!8},
  store/.style={draw,minimum width=3cm,minimum height=0.7cm,align=center,font=\scriptsize,fill=gray!10},
  arr/.style={-{Stealth[length=2.2mm]},thick},
  lab/.style={font=\scriptsize,fill=white,inner sep=1pt}
]
\node[ent] (stu) at (-7,3.2) {Student};
\node[ent] (dev) at (-7,0) {ESP32\\Device};
\node[proc] (p1) at (-3.6,1.6) {1.0\\Read \&\\Send Scan};
\node[proc] (p2) at (0.6,1.6) {2.0\\Validate \&\\Record};
\node[store] (d2) at (4.0,1.6) {D2 Attendance};
\node[proc] (p4) at (7.2,3.4) {4.0\\Reports \&\\Dashboard};
\node[ent] (tea) at (11,3.4) {Teacher};
\node[store] (d1) at (-1.6,-1.8) {D1 Students/Cards};
\node[store] (d3) at (2.6,-1.8) {D3 Courses/Sessions};
\node[proc] (p3) at (6.4,-3.6) {3.0\\Manage\\Data};
\node[ent] (adm) at (10.6,-3.6) {Admin};
\draw[arr] (stu) -- node[lab]{tap} (p1);
\draw[arr] (dev) -- node[lab]{UID} (p1);
\draw[arr] (p1) -- node[lab]{scan event} (p2);
\draw[arr] (d1) -- node[lab,pos=0.45]{student info} (p2);
\draw[arr] (d3) -- node[lab,pos=0.55]{active session} (p2);
\draw[arr] (p2) -- node[lab]{record} (d2);
\draw[arr] (p2) to[bend left=35] node[lab,pos=0.5]{result} (dev);
\draw[arr] (d2) -- node[lab]{records} (p4);
\draw[arr] (p4) -- node[lab]{reports} (tea);
\draw[arr] (tea) to[bend left=25] node[lab,pos=0.55]{session control} (p3);
\draw[arr] (adm) -- node[lab]{master data} (p3);
\draw[arr] (p3) -- node[lab]{store} (d1);
\draw[arr] (p3) -- node[lab]{store} (d3);
\end{tikzpicture}}
\caption{DFD Level 1.}
\end{figure}
 
\subsection{Use Case Diagram}
\begin{figure}[H]
\centering
\begin{tikzpicture}[
  actor/.style={draw,rectangle,minimum width=1.8cm,minimum height=0.8cm,font=\small,fill=yellow!15,align=center},
  uc/.style={draw,ellipse,minimum width=3.4cm,minimum height=0.8cm,font=\scriptsize,align=center,fill=blue!6},
  assoc/.style={thick}
]
\draw[thick,rounded corners] (-3.4,-5.7) rectangle (3.4,4.4);
\node[font=\small\bfseries] at (0,4.0) {IoT Attendance System};
\node[uc] (u1) at (0,3.1) {Scan RFID / QR};
\node[uc] (u2) at (0,2.1) {Validate and Record Attendance};
\node[uc] (u3) at (0,1.1) {Sync Offline Scans};
\node[uc] (u4) at (0,0.1) {Start / End Class Session};
\node[uc] (u5) at (0,-0.9) {View Live Attendance};
\node[uc] (u6) at (0,-1.9) {Generate / Export Report};
\node[uc] (u7) at (0,-2.9) {Manage Students and Cards};
\node[uc] (u8) at (0,-3.9) {Manage Courses};
\node[uc] (u9) at (0,-4.9) {Register Device};
\node[actor] (stu) at (-6,2.6) {Student};
\node[actor] (dev) at (6,2.1) {ESP32\\Device};
\node[actor] (tea) at (-6,-1.0) {Teacher};
\node[actor] (adm) at (6,-3.9) {Admin};
\draw[assoc] (stu) -- (u1);
\draw[assoc] (dev) -- (u1);
\draw[assoc] (dev) -- (u2);
\draw[assoc] (dev) -- (u3);
\draw[assoc] (tea) -- (u4);
\draw[assoc] (tea) -- (u5);
\draw[assoc] (tea) -- (u6);
\draw[assoc] (adm) -- (u7);
\draw[assoc] (adm) -- (u8);
\draw[assoc] (adm) -- (u9);
\draw[assoc] (adm) -- (u6);
\end{tikzpicture}
\caption{Use case diagram.}
\end{figure}
 
\subsection{Activity Diagram}
\begin{figure}[H]
\centering
\begin{tikzpicture}[
  node distance=0.75cm,
  act/.style={draw,rounded corners=6pt,minimum width=3.6cm,minimum height=0.7cm,font=\scriptsize,align=center,fill=blue!6},
  dec/.style={draw,diamond,aspect=2.6,font=\tiny,align=center,inner sep=1pt,fill=orange!12},
  arr/.style={-{Stealth[length=2.2mm]},thick},
  lab/.style={font=\tiny}
]
\node[circle,fill=black,minimum size=0.35cm] (s) {};
\node[act,below=of s] (a1) {Student taps card / shows QR};
\node[act,below=of a1] (a2) {ESP32 reads UID and adds timestamp};
\node[dec,below=of a2] (d0) {Wi-Fi\\connected?};
\node[act,below=of d0] (a3) {Send scan to server};
\node[dec,below=of a3] (d1) {Student\\enrolled?};
\node[dec,below=of d1] (d2) {Already\\marked?};
\node[act,below=of d2] (a4) {Save record (Present / Late)};
\node[act,below=of a4] (a5) {Return success to device};
\node[act,below=of a5] (a6) {OLED name, green LED, 1 beep};
\node[circle,draw,thick,minimum size=0.4cm,below=of a6] (e) {};
\node[circle,fill=black,minimum size=0.22cm] at (e) {};
\node[act,right=2.8cm of d0,minimum width=3.4cm] (b1) {Buffer scan in flash};
\node[act,right=2.8cm of d1,minimum width=3.4cm] (r1) {Reject: not enrolled};
\node[act,right=2.8cm of d2,minimum width=3.4cm] (r2) {Reject: duplicate};
\node[act,right=2.8cm of a5,minimum width=3.4cm] (r3) {Red LED, 2 beeps};
\draw[arr] (s) -- (a1);
\draw[arr] (a1) -- (a2);
\draw[arr] (a2) -- (d0);
\draw[arr] (d0) -- node[lab,right]{yes} (a3);
\draw[arr] (a3) -- (d1);
\draw[arr] (d1) -- node[lab,right]{yes} (d2);
\draw[arr] (d2) -- node[lab,right]{no} (a4);
\draw[arr] (a4) -- (a5);
\draw[arr] (a5) -- (a6);
\draw[arr] (a6) -- (e);
\draw[arr] (d0) -- node[lab,above]{no} (b1);
\draw[arr] (d1) -- node[lab,above]{no} (r1);
\draw[arr] (d2) -- node[lab,above]{yes} (r2);
\draw (r2.east) -- ++(0.5,0);
\draw[arr] (r1.east) -- ++(0.5,0) |- (r3.east);
\draw[arr] (r3.south) |- (e.east);
\draw[arr] (b1.east) -- ++(0.5,0) |- node[lab,pos=0.25,right]{sync later} (a3.east);
\end{tikzpicture}
\caption{Activity diagram for one attendance scan.}
\end{figure}
 
\subsection{Entity Relationship Diagram}
\begin{figure}[H]
\centering
\begin{tikzpicture}[
  ent/.style={draw,rectangle split,rectangle split parts=2,font=\scriptsize,align=left,minimum width=3.2cm,fill=blue!5,rectangle split part fill={blue!20,white}},
  rel/.style={-,thick},
  card/.style={font=\scriptsize\bfseries,fill=white,inner sep=1pt},
  rl/.style={font=\tiny\itshape,fill=white,inner sep=1pt}
]
\node[ent] (teacher) at (-6.6,5) {\textbf{TEACHER}\nodepart{second}\underline{teacher\_id}\\name\\email\\password\_hash};
\node[ent] (course) at (0,5) {\textbf{COURSE}\nodepart{second}\underline{course\_id}\\code\\title\\teacher\_id (FK)};
\node[ent] (student) at (-6.6,0.4) {\textbf{STUDENT}\nodepart{second}\underline{student\_id}\\name\\roll\_no\\rfid\_uid\\email};
\node[ent] (enroll) at (0,0.4) {\textbf{ENROLLMENT}\nodepart{second}\underline{enrollment\_id}\\student\_id (FK)\\course\_id (FK)};
\node[ent] (session) at (5.4,5) {\textbf{CLASS\_SESSION}\nodepart{second}\underline{session\_id}\\course\_id (FK)\\start\_time\\end\_time\\status};
\node[ent] (att) at (5.4,0.4) {\textbf{ATTENDANCE\_RECORD}\nodepart{second}\underline{record\_id}\\student\_id (FK)\\session\_id (FK)\\device\_id (FK)\\scan\_time\\status};
\node[ent] (device) at (5.4,-3.8) {\textbf{DEVICE}\nodepart{second}\underline{device\_id}\\mac\_address\\classroom\\last\_seen};
\draw[rel] (teacher) -- node[card,near start,above]{1} node[card,near end,above]{N} node[rl,midway,above=2pt]{teaches} (course);
\draw[rel] (course) -- node[card,near start,above]{1} node[card,near end,above]{N} node[rl,midway,above=2pt]{has} (session);
\draw[rel] (student) -- node[card,near start,above]{1} node[card,near end,above]{N} node[rl,midway,above=2pt]{makes} (enroll);
\draw[rel] (course) -- node[card,near start,right]{1} node[card,near end,right]{N} node[rl,midway,right]{enrolled in} (enroll);
\draw[rel] (session) -- node[card,near start,right]{1} node[card,near end,right]{N} node[rl,midway,right]{records} (att);
\draw[rel] (device) -- node[card,near start,right]{1} node[card,near end,right]{N} node[rl,midway,right]{captures} (att);
\draw[rel] (student.south) -- ++(0,-1.5) -| node[card,near start,below]{1} node[card,near end,left]{N} node[rl,pos=0.45,below]{is marked in} (att.west);
\end{tikzpicture}
\caption{Entity relationship diagram (crow's-foot style cardinalities shown as 1 and N).}
\end{figure}
 
\subsection{Class Diagram}
\begin{figure}[H]
\centering
\begin{tikzpicture}[
  cls/.style={draw,rectangle split,rectangle split parts=3,font=\tiny,align=left,minimum width=3.0cm,fill=blue!4},
  rel/.style={-,thick},
  asc/.style={-{Stealth[length=2mm]},thick},
  m/.style={font=\tiny\bfseries,fill=white,inner sep=0.5pt}
]
\node[cls] (student) at (-5.4,5.6) {\textbf{Student}\nodepart{second}-studentId: int\\-name: String\\-rollNo: String\\-rfidUid: String\nodepart{third}+getName(): String\\+matchesUid(uid): boolean};
\node[cls] (course) at (0,5.6) {\textbf{Course}\nodepart{second}-courseId: int\\-code: String\\-title: String\\-students: List<Student>\nodepart{third}+enroll(s: Student)\\+isEnrolled(s): boolean};
\node[cls] (teacher) at (5.4,5.6) {\textbf{Teacher}\nodepart{second}-teacherId: int\\-name: String\\-email: String\nodepart{third}+startSession(c: Course)\\+endSession()};
\node[cls] (session) at (2.7,1.6) {\textbf{ClassSession}\nodepart{second}-sessionId: int\\-course: Course\\-startTime: LocalDateTime\\-active: boolean\nodepart{third}+open()\\+close()\\+isLate(t): boolean};
\node[cls] (record) at (-2.7,1.6) {\textbf{AttendanceRecord}\nodepart{second}-recordId: int\\-student: Student\\-session: ClassSession\\-scanTime: LocalDateTime\\-status: Status\nodepart{third}+getStatus(): Status};
\node[cls] (scan) at (-5.4,-2.4) {\textbf{ScanEvent}\nodepart{second}-uid: String\\-deviceId: String\\-timestamp: LocalDateTime\nodepart{third}+toJson(): String};
\node[cls] (device) at (-0.2,-2.4) {\textbf{Device}\nodepart{second}-deviceId: String\\-classroom: String\\-online: boolean\nodepart{third}+scan(uid): ScanEvent\\+showResult(msg)};
\node[cls] (service) at (5.4,-2.4) {\textbf{AttendanceService}\nodepart{second}-records: List<AttendanceRecord>\nodepart{third}+process(e: ScanEvent): Result\\+getReport(c: Course)\\+exportCsv()};
\draw[rel] (teacher) -- node[m,near start,above]{1} node[m,near end,right]{0..*} (session);
\draw[rel] (course) -- node[m,near start,left]{1} node[m,near end,left]{0..*} (session);
\draw[rel] (student) -- node[m,near start,above]{0..*} node[m,near end,above]{0..*} node[font=\tiny\itshape,midway,above]{enrolled} (course);
\draw[rel] (student) -- node[m,near start,left]{1} node[m,near end,left]{0..*} (record);
\draw[rel] (session) -- node[m,near start,above]{1} node[m,near end,above]{0..*} (record);
\draw[asc] (device) -- node[m,midway,above]{creates} (scan);
\draw[asc] (service) -- node[m,midway,right]{creates} (record);
\draw[asc] (scan.south) -- ++(0,-1.1) -| node[m,near start,below]{processed by} (service.south);
\end{tikzpicture}
\caption{Class diagram.}
\end{figure}
 
\subsection{Object Diagram}
\begin{figure}[H]
\centering
\begin{tikzpicture}[
  obj/.style={draw,rectangle split,rectangle split parts=2,font=\scriptsize,align=left,minimum width=3.2cm,fill=green!6},
  rel/.style={-,thick},
  lab/.style={font=\tiny\itshape,fill=white,inner sep=1pt}
]
\node[obj] (s1) at (-5,3) {\underline{\textbf{s1 : Student}}\nodepart{second}name = ``Md Jahidul Islam''\\rollNo = ``IT24018''\\rfidUid = ``A3F29C1B''};
\node[obj] (c1) at (0,3) {\underline{\textbf{c1 : Course}}\nodepart{second}code = ``SEPM-401''\\title = ``Software Eng. \& PM''};
\node[obj] (t1) at (5,3) {\underline{\textbf{t1 : Teacher}}\nodepart{second}name = ``Course Teacher''};
\node[obj] (ses1) at (2.5,-0.2) {\underline{\textbf{ses1 : ClassSession}}\nodepart{second}startTime = 09:00\\active = true};
\node[obj] (r1) at (-3,-0.2) {\underline{\textbf{r1 : AttendanceRecord}}\nodepart{second}scanTime = 09:03:12\\status = PRESENT};
\node[obj] (e1) at (-5,-3.3) {\underline{\textbf{e1 : ScanEvent}}\nodepart{second}uid = ``A3F29C1B''\\timestamp = 09:03:12};
\node[obj] (d1) at (0,-3.3) {\underline{\textbf{d1 : Device}}\nodepart{second}deviceId = ``ESP32-R101''\\online = true};
\node[obj] (svc) at (5,-3.3) {\underline{\textbf{svc : AttendanceService}}\nodepart{second}records = [r1]};
\draw[rel] (s1) -- node[lab]{enrolled} (c1);
\draw[rel] (t1) -- node[lab]{starts} (ses1);
\draw[rel] (c1) -- node[lab]{has} (ses1);
\draw[rel] (s1) -- node[lab]{marked} (r1);
\draw[rel] (r1) -- node[lab]{in} (ses1);
\draw[rel] (d1) -- node[lab]{creates} (e1);
\draw[rel] (e1) to[bend right=15] node[lab]{processed by} (svc);
\draw[rel] (svc) -- node[lab]{holds} (r1);
\end{tikzpicture}
\caption{Object diagram for one successful scan.}
\end{figure}
 
%=====================================================================
\section{Prototype}
\subsection{Prototype Goal}
The first prototype demonstrates the core logic without needing hardware. A \texttt{Device} class simulates the ESP32: pressing a button in a JavaFX window (or typing a card UID) creates a \texttt{ScanEvent}, which \texttt{AttendanceService} validates and stores as an \texttt{AttendanceRecord}. The real ESP32 later replaces the simulator and sends the same JSON message, so no server logic changes.
 
\subsection{Classes Used (from the Class Diagram)}
\begin{table}[H]
\centering
\small
\begin{tabular}{lp{9.5cm}}
\toprule
\textbf{Class} & \textbf{Responsibility in the prototype}\\
\midrule
\texttt{Student} & Holds name, roll number and RFID UID\\
\texttt{Course} & Holds the enrolled students and checks enrolment\\
\texttt{ClassSession} & Open/close state and late-time rule\\
\texttt{ScanEvent} & One scan message from a device\\
\texttt{Device} & Simulated ESP32 that produces scan events\\
\texttt{AttendanceRecord} & Stored result of a valid scan\\
\texttt{AttendanceService} & Validation, duplicate check, storage and report\\
\texttt{MainApp (JavaFX)} & Simple interface with UID field, Scan button, live table\\
\bottomrule
\end{tabular}
\caption{Prototype classes.}
\end{table}
 
\subsection{Interface Layout}
\begin{figure}[H]
\centering
\begin{tikzpicture}
\draw[thick,rounded corners] (0,0) rectangle (11,6.4);
\fill[blue!15] (0,5.6) rectangle (11,6.4);
\node[anchor=west,font=\small\bfseries] at (0.2,6.0) {IoT Attendance -- Prototype};
\node[anchor=west,font=\scriptsize] at (0.3,5.1) {Course:};
\draw (1.3,4.9) rectangle (4.0,5.3); \node[font=\scriptsize] at (2.65,5.1) {SEPM-401};
\draw[fill=green!20] (4.4,4.9) rectangle (6.2,5.3); \node[font=\scriptsize] at (5.3,5.1) {Start Session};
\draw[fill=red!15] (6.4,4.9) rectangle (8.2,5.3); \node[font=\scriptsize] at (7.3,5.1) {End Session};
\node[anchor=west,font=\scriptsize] at (0.3,4.2) {Device: ESP32-R101 (simulated)};
\node[anchor=west,font=\scriptsize] at (0.3,3.6) {Card UID:};
\draw (1.5,3.4) rectangle (5.0,3.8); \node[font=\scriptsize] at (3.25,3.6) {A3F29C1B};
\draw[fill=orange!20] (5.3,3.4) rectangle (6.8,3.8); \node[font=\scriptsize] at (6.05,3.6) {Scan};
\node[anchor=west,font=\scriptsize] at (7.2,3.6) {Result: PRESENT};
\draw (0.3,0.4) rectangle (10.7,3.0);
\fill[gray!20] (0.3,2.6) rectangle (10.7,3.0);
\node[font=\tiny\bfseries] at (1.6,2.8) {Roll};
\node[font=\tiny\bfseries] at (4.0,2.8) {Name};
\node[font=\tiny\bfseries] at (7.0,2.8) {Time};
\node[font=\tiny\bfseries] at (9.4,2.8) {Status};
\node[font=\tiny] at (1.6,2.3) {IT24018};
\node[font=\tiny] at (4.0,2.3) {Md Jahidul Islam};
\node[font=\tiny] at (7.0,2.3) {09:03:12};
\node[font=\tiny] at (9.4,2.3) {PRESENT};
\end{tikzpicture}
\caption{Planned prototype window (wireframe).}
\end{figure}
 
\subsection{Core Code (excerpt)}
\begin{lstlisting}[style=java,caption={Key classes of the prototype (simplified).}]
public class Student {
    private final int studentId;
    private final String name, rollNo, rfidUid;
    public Student(int id, String name, String rollNo, String uid) {
        this.studentId = id; this.name = name;
        this.rollNo = rollNo; this.rfidUid = uid;
    }
    public boolean matchesUid(String uid) { return rfidUid.equalsIgnoreCase(uid); }
    public String getName() { return name; }
    public String getRollNo() { return rollNo; }
}
 
public class ScanEvent {
    private final String uid, deviceId;
    private final java.time.LocalDateTime timestamp;
    public ScanEvent(String uid, String deviceId) {
        this.uid = uid; this.deviceId = deviceId;
        this.timestamp = java.time.LocalDateTime.now();
    }
    public String getUid() { return uid; }
    public java.time.LocalDateTime getTimestamp() { return timestamp; }
}
 
public class Device {
    private final String deviceId;
    public Device(String deviceId) { this.deviceId = deviceId; }
    public ScanEvent scan(String uid) { return new ScanEvent(uid, deviceId); }
}
 
public class AttendanceService {
    private final java.util.List<AttendanceRecord> records = new java.util.ArrayList<>();
    public String process(ScanEvent e, Course course, ClassSession session) {
        if (!session.isActive()) return "NO ACTIVE SESSION";
        Student s = course.findByUid(e.getUid());
        if (s == null) return "REJECTED: NOT ENROLLED";
        for (AttendanceRecord r : records)
            if (r.getStudent() == s && r.getSession() == session)
                return "REJECTED: DUPLICATE";
        String status = session.isLate(e.getTimestamp()) ? "LATE" : "PRESENT";
        records.add(new AttendanceRecord(s, session, e.getTimestamp(), status));
        return status;
    }
}
\end{lstlisting}
 
\subsection{Message Format (Device to Server)}
\begin{lstlisting}[basicstyle=\ttfamily\footnotesize,frame=single,caption={MQTT payload on topic attendance/scan.}]
{ "uid": "A3F29C1B", "deviceId": "ESP32-R101", "ts": "2026-11-02T09:03:12" }
\end{lstlisting}
 
\subsection{Repository Structure}
\begin{lstlisting}[basicstyle=\ttfamily\footnotesize,frame=single]
iot-attendance/
  docs/            proposal.tex, diagrams, gantt
  prototype-java/  src/ (Student, Course, ClassSession, ScanEvent,
                   Device, AttendanceRecord, AttendanceService, MainApp)
  firmware-esp32/  attendance.ino
  server/          Javalin API, SQL schema
  README.md
\end{lstlisting}
 
%=====================================================================
\section{Testing Plan}
\begin{table}[H]
\centering
\small
\begin{tabular}{llp{6.2cm}}
\toprule
\textbf{ID} & \textbf{Type} & \textbf{Test case and expected result}\\
\midrule
T1 & Unit & Valid UID returns PRESENT and one record is stored\\
T2 & Unit & Unknown UID returns REJECTED: NOT ENROLLED\\
T3 & Unit & Second scan of same card in a session returns DUPLICATE\\
T4 & Unit & Scan after the late threshold returns LATE\\
T5 & Integration & ESP32 scan reaches server and OLED shows result in under 2\,s\\
T6 & System & Wi-Fi off for 10 minutes: scans are buffered and synced afterwards\\
T7 & Security & Request without valid device credentials is refused\\
\bottomrule
\end{tabular}
\caption{Test cases.}
\end{table}
 
%=====================================================================
\section{Conclusion}
This proposal defines an IoT-based attendance system that combines an ESP32 RFID/QR device with a Java server and dashboard. The Iterative-Incremental plan delivers a working simulated prototype first and the real hardware next, which keeps the risk low. The schedule, milestones, deliverables, Gantt chart, six UML diagrams and the Java prototype design give a complete basis for implementation in the following weeks.
 
\begin{thebibliography}{9}
\bibitem{esp32} Espressif Systems, \emph{ESP32 Series Datasheet}, \url{https://www.espressif.com}.
\bibitem{mfrc522} NXP Semiconductors, \emph{MFRC522 Contactless Reader IC Datasheet}, \url{https://www.nxp.com}.
\bibitem{mqtt} OASIS, \emph{MQTT Version 3.1.1 Specification}.
\bibitem{sommerville} I. Sommerville, \emph{Software Engineering}, Pearson.
\bibitem{pressman} R. S. Pressman, \emph{Software Engineering: A Practitioner's Approach}, McGraw-Hill.
\bibitem{fowler} M. Fowler, \emph{UML Distilled}, Addison-Wesley.
\end{thebibliography}
 
\end{document}

 
