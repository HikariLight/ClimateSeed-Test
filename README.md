# unit-conversion
A technical test for my ClimateSeed fullstack developer job application.  
Build a unit conversion SPA app.  
[Link](https://dhia-climateseed-test.netlify.app/)

## Requirements

Using Vue.js, implement a form with :
- Quantity
- Unit from (text input)
- Unit to (text input)
- Submit button

The purpose of this app would be to convert any unit to another using those only those 4 arrays (From, To, Conversion factor):
- ["lb", "g", "453.59237"],  
- ["lb", "kg", "0.45359237"],  
- ["kg", "lb", "2.20462262"],  
- ["kg", "metric ton", "0.001"]  

The user should also be able to convert lb to metric ton based on the ratios given above.  
The user should be able to view the result on the page.  

NB: every combination of conversion between “lb”, “g”, “kg” and “metric ton” should be possible.