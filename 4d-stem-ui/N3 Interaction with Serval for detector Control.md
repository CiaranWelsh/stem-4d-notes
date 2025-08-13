

_(Expose only the detector settings a 4D‑STEM user really needs while hiding the rest.)_


#### 4Q Drill‑down

- **What?**  
    A thin integration layer that automatically connects the 4D‑STEM UI to Serval. Serval should not be visible to the user. Users define dacs settings, bpc configuration and destination parameters via luna-stem-ui which forwards them onto serval (via serval-client in rust). Any detector control parameters are invisible to user, unless absolutely necessary (?). 
    
- **When/where?**  
    • Connection: on application start, begin trying to connect to serval, keep retrying every 1s interval until handshake initiated. 
    
- **How well?**
    
    - Auto‑connect (or reconnect) ≤ 1 s mean time.
        
    - UI exposes **≤ 3** Serval controls (Bias [V], Acquisition Mode, Hardware Mask).
        
    - 100 % of forwarded commands receive an ACK/ERR within 200 ms round‑trip.
        
    - Lost connection during an active scan pauses acquisition ≤ 100 ms and resumes ≤ 1 s after link recovery.
    - Need to start measurement and scan within < 1ms to ensure proper sync. 
        
- **How verified?**  
    gRPC stub tests + ζ‑service fault‑injection harness measuring timing; UI snapshot test for visible controls.