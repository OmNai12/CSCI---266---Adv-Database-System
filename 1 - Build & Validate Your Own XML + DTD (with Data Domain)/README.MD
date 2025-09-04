# Hiking Trails Data Validation

## Decision
Which hiking trails are open and safe for visitors today?

## Signals
1. Trail name [Trails]
2. Location [Trails/Maps]
3. Distance in miles [Metrics]
4. Elevation gain in feet (optional) [Metrics]
5. Difficulty level (easy/moderate/hard) [Trails]
6. Trail condition (open/closed/snow/caution) [Safety]
7. Last reported date [Trails]

## Gaps
No check on weather or seasonal closures; no real-time hazard data.

## Risk
If wrong or stale, hikers may end up on closed/dangerous trails, risking safety.

## Stewardship
Trail data should be public, updated frequently by park services. Archives retained for 1 year for reference.

## Data Domain
Primary: [Hiking Trails]  
Adjacent: [Weather], [Maps]

---
My broken.xml fails because it omits the required `<location>` element.
