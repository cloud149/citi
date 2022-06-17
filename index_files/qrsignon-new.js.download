var environment = window.location.href;
flag = false;

if (environment.indexOf("localhost") != -1  || environment.indexOf("dit") != -1  || environment.indexOf("uat") != -1 || environment.indexOf("sit") != -1 || environment.indexOf("webqa") != -1) {
    var tswebClientId = "6e919cb7-cdeb-4035-87e8-e55bd4c49c05";
    var tswebEndPoint = "https://sit.api.citi.com/gcgapi/dev1/ts-web";
    var tswebAppId = "169928-citibankonline-websdk-nam";
} else {
    var tswebClientId = "4a51fb19-a1a7-4247-bc7e-18aa56dd1c40";
    var tswebEndPoint = "https://api.citi.com/gcgapi/prod/ts-web";
    var tswebAppId = "169928-citibankonline-websdk-nam";
}

if(sessionStorage.getItem('qrUsername')){
    sessionStorage.removeItem('qrUsername');
}
if(sessionStorage.getItem('qrPassword')){
    sessionStorage.removeItem('qrPassword');
}
let mSDK = com.ts.mobile.sdk;
var qrCode = false;
let total_time = 45000;
function move(){
    var elemTR = document.getElementById("quadrant_two");
    var elemBR = document.getElementById("quadrant_four");
    var elemBL = document.getElementById("quadrant_three");
    var elemTL = document.getElementById("quadrant_one");

   // console.log("resetting settings");
    //initial settings for accounting for tab switching
    var prevState = "visible";
    var timeElapsed = 0; 

    //initial settings for the animation of the topRightQuadrant
    elemTR.style.height = 8 + "px";
    elemTR.style.width = 0 + "px";
    var heightTR = 8; 
    var widthTR = 0;

    //initial settings for the animation of the bottomRightQuadrant
    elemBR.style.left = 128 + "px";
    elemBR.style.width = 8 + "px";
    elemBR.style.height = "0";
    var heightBR = 0; 
    var widthBR = 8; 
    var leftBR = 128; 

    //initial settings for the animation of the bottomLeftQuadrant
    elemBL.style.left = 136 + "px";
    elemBL.style.top = 128 + "px";
    elemBL.style.width = "0";
    elemBL.style.height = 8 + "px";
    var heightBL = 8; 
    var widthBL = 0; 
    var leftBL = 136; 
    var topBL = 128; 

    //initial settings for the animation of the topLeftQuadrant
    elemTL.style.top = 136 + "px";
    elemTL.style.width = 8 + "px";
    elemTL.style.height = "0";
    var heightTL = 0; 
    var widthTL = 8; 
    var topTL = 136; 

    //function for calculating amount of border accumulated while away
    //initBorder is the amount of border that had accumulated prior to tab switching
    //time is the amount of time that was clicked away
    //init_quadrant is the starting quadrant that the snake was in when the tab was first switched away from the modal
    function accumulatedArea(initBorder, time, init_quadrant){
      var change_in_border = 4*(136+136)*(136+136)*4*(136+136)*(136+136)*(time)/(total_time*total_time);
      var borderAccumulated = initBorder+ change_in_border;
      //for third quadrant on the horizontal portion, would expect it to be between 544 and 680
      //console.log("initBorder: " + String(initBorder));
      //change_in_border should be <136
      //console.log("change_in_border: "+String(change_in_border));
      const updatedLocation = new Map([
        ["updated_quadrant", 0],
        ["updated_height", 0],
        ["updated_width", 0]
      ]);
      //console.log("borderAccumulated: "+String(borderAccumulated));
      //situation where so much time had passed that a new iteration has started:
      if(borderAccumulated >= 8*136){
        //number of quadrants to advance past the start (center of the top horizontal line)
        num_quad_adv = ~~((borderAccumulated-8*136)/(136+136));
        updatedLocation.set("updated_quadrant", num_quad_adv+1);
        //"overhang" height and width...height and width that extends past any completed quadrants
        new_height_and_width = (borderAccumulated-(8*136))-(num_quad_adv*2*136);

        //console.log("borderAccumulated is greater than 8*136");
        //console.log("num_quad_advance: "+String(num_quad_adv));
        //console.log("updated_quadrant: "+String(num_quad_adv+1));
        //console.log("new_height_and_width: "+String(new_height_and_width));
        if(num_quad_adv+1 == 1 || num_quad_adv+1 == 3){
          //console.log("we think the final location is in quadrant 1 or 3");
          if(new_height_and_width > 136){
            clearInterval(id);
            updatedLocation.set("updated_width", 136);
            updatedLocation.set("updated_height", new_height_and_width-136);
          }
          else{
            clearInterval(id);
            updatedLocation.set("updated_width", new_height_and_width);
            updatedLocation.set("updated_height", 8);
          }
        }
        else{
          //console.log("we think the final location is in quadrant 1 or 3");
          if(new_height_and_width > 136){
            clearInterval(id);
            updatedLocation.set("updated_height", 136);
            updatedLocation.set("updated_width", new_height_and_width-136);
          }
          else{
            clearInterval(id);
            updatedLocation.set("updated_height", new_height_and_width);
            updatedLocation.set("updated_width", 8);
          }
        }
      clearInterval(id);
      }
      //situation where so much time has passed that 2 new iterations had started
      else if(borderAccumulated > 2*8*136){
        num_quad_adv = ~~((borderAccumulated-2*8*136)/(136+136));

        new_height_and_width = (borderAccumulated-(2*8*136))-(num_quad_adv*2*136);
        //console.log("borderAccumulated is greater than 2*8*136");
        if(num_quad_adv+1 == 1 || num_quad_adv == 3){
          //console.log("we think the final location is in quadrant 1 or 3");
          if(new_height_and_width > 136){
            updatedLocation.set("updated_width", 136);
            updatedLocation.set("updated_height", new_height_and_width-136);
          }
          else{
            updatedLocation.set("updated_width", new_height_and_width);
            updatedLocation.set("updated_height", 8);
          }
        }
        else{
          //console.log("we think the final location is in quadrant 2 or 4");
          if(new_height_and_width > 136){
            updatedLocation.set("updated_height", 136);
            updatedLocation.set("updated_width", new_height_and_width-136);
          }
          else{
            updatedLocation.set("updated_height", new_height_and_width);
            updatedLocation.set("updated_width", 8);
          }
        }

        clearInterval(id);
      }
      else{
        //now borderAccumulated refers only to anything that was accumulated within a single iteration
        //calculate the number of quadrants to advance by using integer division
        //console.log("accumulated border: "+String(borderAccumulated)); //379
        //num_quad_adv = ~~(change_in_border/(2*136));
        num_quad_adv = ~~(borderAccumulated/(2*init_quadrant*136));
        //console.log("num_quad_adv: " + String(num_quad_adv));
        updatedLocation.set("updated_quadrant", init_quadrant + num_quad_adv);

        //now only need the amount of "overhang" (amts of height and width that extend past completed quadrants)
        new_height_and_width = borderAccumulated-(num_quad_adv+init_quadrant-1)*(136+136);
        //console.log("remaining height and width on the updated quadrant: " + String(new_height_and_width));
        if(init_quadrant+num_quad_adv == 1 || init_quadrant+num_quad_adv == 3){
          //console.log("decided the updated quadrant is either the 1st or the third");
          //console.log("init_quad: "+String(init_quadrant));
          //console.log("num_quad_adv: " + String(num_quad_adv));
          //console.log("new_height_and_width: "+String(new_height_and_width));
          if(new_height_and_width > 136){
            //console.log("should be in the ")
            updatedLocation.set("updated_width", 136);
            updatedLocation.set("updated_height", new_height_and_width-136);
            //updatedLocation.set("updated_height", new_height_and_width);
            //console.log("updated_height: "+ String(new_height_and_width-136));
          }else{
            updatedLocation.set("updated_width", new_height_and_width);
            updatedLocation.set("updated_height", 8);
            //console.log("I think it's supposed to be a horizontal line");
          }
        }else{
          // console.log("decided the updated quadrant is either the 2nd or the fourth");
          // console.log("init_quad: "+String(init_quadrant));
          // console.log("num_quad_adv: " + String(num_quad_adv));
          // console.log("new_height_and_width: "+String(new_height_and_width));
          if(new_height_and_width > 136){
            // console.log("I think it's supposed to be a horizontal line");
            updatedLocation.set("updated_width", new_height_and_width-136);
            updatedLocation.set("updated_height", 136);
          }
          else{
            // console.log("I think it's supposed to be a vertical line");
            updatedLocation.set("updated_width", 8);
            updatedLocation.set("updated_height", new_height_and_width);
          }
        }
      }
      return updatedLocation;
    }

    //every 4*(137+136) milliseconds we are executing function quadrant manager, which controls all 4 quadrants
    //if visible, a pixal is accrued every total_time/(4*(136+136)) ms
    //if visible, a pixel is accrued every 41.3602952 ms
    var id = setInterval(quadrant_manager, total_time/(4*(136+136)));    
    function quadrant_manager(){
      console.log("entering quadrant manager: ");
      // console.log("heightTR: " + String(heightTR));
      // console.log("widthTR: " + String(widthTR));
      // console.log("heightBR: " + String(heightBR));
      // console.log("widthBR: " + String(widthBR));
      // console.log("heightBL: " + String(heightBL));
      // console.log("widthBL: " + String(widthBL));
      // console.log("heightTL: " + String(heightTL));
      // //console.log("entering quadrant manager: ");
      // console.log("widthTL: " + String(widthTL));
      // console.log("elemTL.style.width: "+String(elemTL.style.width));
      if(document.visibilityState === "visible"){
        //console.log("visible");
        //console.log(heightTR);
        //console.log(widthTR);
        if(prevState === "hidden"){
          //console.log("was hidden");
          console.log("timeElapsed: " + String(timeElapsed)); 
          //console.log("ElemBR height: "+String(elemBR.style.height));
          //console.log("possible accumulated area: "+String(4*(136+136)*4*(136+136)*timeElapsed/(total_time*total_time))); //presumed accumulated area
          //console.log("calculation from before: " + String((4*(136+136)*(136+136)*4*(136+136)*(136+136)*(timeElapsed))/(total_time*total_time)));
          //how do account for the fact that every other border has a width or height of 8 even though they haven't started animating yet? 
          if(widthTL > 8){
            //console.log("starting out horizontal in quadrant 4");
            // console.log("heightTR: " + String(heightTR));
            // console.log("widthTR: " + String(widthTR));
            // console.log("heightBR: " + String(heightBR));
            // console.log("widthBR: " + String(widthBR));
            // console.log("heightBL: " + String(heightBL));
            // console.log("widthBL: " + String(widthBL));
            // console.log("heightTL: " + String(heightTL));
            // console.log("widthTL: " + String(widthTL));
            updated_pos = accumulatedArea((136*7)+widthTL, timeElapsed, 4);
          }
          else if(heightTL > 0){
            //console.log("starting out vertical in quadrant 4");
            updated_pos = accumulatedArea((136*6)+heightTL, timeElapsed, 4);
          }
          else if(heightBL > 8){
            //console.log("starting out vertical in quadrant 3");
            updated_pos = accumulatedArea((136*5) + heightBL, timeElapsed, 3);
          }
          else if(widthBL > 0){
            //console.log("starting out horizontal in quadrant 3");
            updated_pos = accumulatedArea((136*4)+widthBL, timeElapsed, 3);
          }
          else if(widthBR > 8){
            //console.log("starting on the horizontal line of the second quadrant");
            updated_pos = accumulatedArea((136*3)+widthBR, timeElapsed, 2);
          }
          else if(heightBR > 0){
            //console.log("starting on the vertical line of the second quadrant");
            updated_pos = accumulatedArea((136*2)+heightBR, timeElapsed, 2);
          }
          else if(heightTR > 8){
            //console.log("starting on the vertical line of the first quadrant");
            updated_pos = accumulatedArea(136+heightTR, timeElapsed, 1);
          }
          else{
            //console.log("starting on the horizontal line of the first quadrant");
            updated_pos = accumulatedArea(widthTR, timeElapsed, 1);
            //console.log("it was in the first quadrant!");
          }
          updated_quadrant = updated_pos.get("updated_quadrant");
          if(updated_quadrant == 1){
            //console.log("updating to first quadrant!");
            elemTR.style.height = updated_pos.get("updated_height") + "px";
            elemTR.style.width = updated_pos.get("updated_width") + "px";
            heightTR = updated_pos.get("updated_height"); 
            widthTR = updated_pos.get("updated_width");

            elemBR.style.left = 128 + "px";
            elemBR.style.width = 8 + "px";
            elemBR.style.height = "0";
            heightBR = 0; 
            widthBR = 8; 
            leftBR = 128;

            elemBL.style.left = 136 + "px";
            elemBL.style.top = 128 + "px";
            elemBL.style.width = "0";
            elemBL.style.height = 8 + "px";
            heightBL = 8; 
            widthBL = 0; 
            leftBL = 136; 
            topBL = 128; 

            elemTL.style.top = 136 + "px";
            elemTL.style.width = 8 + "px";
            elemTL.style.height = 0 + "px";
            heightTL = 0; 
            widthTL = 8; 
            topTL = 136; 

            //console.log("landed in quadrant 1");
            //console.log("widthTL: " + String(widthTL));
            //console.log("elemTL.style.width: "+String(elemTL.style.width));
          }
          else if(updated_quadrant == 2){
            elemTR.style.height = 136 + "px";
            elemTR.style.width = 136 + "px";
            heightTR = 136; 
            widthTR = 136;

            elemBR.style.left = 136-updated_pos.get("updated_width") + "px";
            elemBR.style.width = updated_pos.get("updated_width") + "px";
            elemBR.style.height = updated_pos.get("updated_height") + "px";;
            heightBR = updated_pos.get("updated_height"); 
            widthBR = updated_pos.get("updated_width"); 
            leftBR = 136-updated_pos.get("updated_width");

            elemBL.style.left = 136 + "px";
            elemBL.style.top = 128 + "px";
            elemBL.style.width = "0";
            elemBL.style.height = 8 + "px";
            heightBL = 8; 
            widthBL = 0; 
            leftBL = 136; 
            topBL = 128; 

            elemTL.style.top = 136 + "px";
            elemTL.style.width = 8 + "px";
            elemTL.style.height = 0 + "px";
            heightTL = 0; 
            widthTL = 8; 
            topTL = 136; 

            //console.log("landed in quadrant 2");
            //console.log("widthTL: " + String(widthTL));
            //console.log("elemTL.style.width: "+String(elemTL.style.width));
          }
          else if(updated_quadrant == 3){
            elemTR.style.height = 136 + "px";
            elemTR.style.width = 136 + "px";
            heightTR = 136; 
            widthTR = 136;

            elemBR.style.left = 0 + "px";
            elemBR.style.width = 136 + "px";
            elemBR.style.height = 136 + "px";;
            heightBR = 136; 
            widthBR = 136; 
            leftBR = 0;

            elemBL.style.left = 136-updated_pos.get("updated_width") + "px";
            elemBL.style.top = 136-updated_pos.get("updated_height") + "px";
            elemBL.style.width = updated_pos.get("updated_width")+"px";
            elemBL.style.height = updated_pos.get("updated_height") + "px";
            heightBL = updated_pos.get("updated_height"); 
            widthBL = updated_pos.get("updated_width"); 
            leftBL = 136-updated_pos.get("updated_width"); 
            topBL = 136-updated_pos.get("updated_height"); 

            elemTL.style.top = 136 + "px";
            elemTL.style.width = 8 + "px";
            elemTL.style.height = 0 + "px";
            heightTL = 0; 
            widthTL = 8; 
            topTL = 136;

            //console.log("landed in quadrant 3 ");
            // console.log("widthTL: " + String(widthTL));
            // console.log("elemTL.style.width: "+String(elemTL.style.width));
          }
          else if(updated_quadrant == 4){
            elemTR.style.height = 136 + "px";
            elemTR.style.width = 136 + "px";
            heightTR = 136; 
            widthTR = 136;

            elemBR.style.left = 0 + "px";
            elemBR.style.width = 136 + "px";
            elemBR.style.height = 136 + "px";;
            heightBR = 136; 
            widthBR = 136; 
            leftBR = 0;

            elemBL.style.left = 0 + "px";
            elemBL.style.top = 0 + "px";
            elemBL.style.width = 136+"px";
            elemBL.style.height = 136+ "px";
            heightBL = 136; 
            widthBL = 136; 
            leftBL = 0; 
            topBL = 0; 

            elemTL.style.top = 136-updated_pos.get("updated_height") + "px";
            elemTL.style.width = updated_pos.get("updated_width")+"px";
            elemTL.style.height = updated_pos.get("updated_height") + "px";
            heightTL = updated_pos.get("updated_height"); 
            widthTL = updated_pos.get("updated_width"); 
            topTL = 136-updated_pos.get("updated_height");

            //console.log("landed in quadrant 4: ");
            // console.log("widthTL: " + String(widthTL));
            // console.log("elemTL.style.width: "+String(elemTL.style.width));
          }
        }
        //console.log("heightTR: " + String(heightTR));
        //console.log("widthTR: "+String(widthTR));
        //animating the topRightQuadrant
        if(heightTR < 136){
          console.log("animating the first quadrant");
          // console.log("widthTL: " + String(widthTL));
          // console.log("elemTL.style.width: "+String(elemTL.style.width));
          if(widthTR < 136){
            widthTR++; 
            elemTR.style.width = widthTR + "px";
          }
          else{
            heightTR++;
            elemTR.style.height = heightTR + "px";
          }
        }
        //animating the bottomRightQuadrant
        else if(heightTR >= 136 && widthBR < 136){
          //console.log("animating the second quadrant");
          // console.log("widthTL: " + String(widthTL));
          // console.log("elemTL.style.width: "+String(elemTL.style.width));
          if(heightBR < 136){
            heightBR++;
            elemBR.style.height = heightBR + "px";
          }
          else{
            widthBR++; 
            leftBR--; 
            elemBR.style.width = widthBR + "px";
            elemBR.style.left = leftBR + "px";
          }
        }
        //animating the bottomLeftQuadrant
        else if(widthBR >= 136 && heightBL < 136){
          //console.log("animating the third quadrant");
          // console.log("widthTL: " + String(widthTL));
          // console.log("elemTL.style.width: "+String(elemTL.style.width));
          if(widthBL < 136){
            widthBL++; 
            leftBL--; 
            elemBL.style.width = widthBL + "px";
            elemBL.style.left = leftBL + "px";
            //console.log("widthBL: "+String(widthBL));
          }
          else{
            heightBL++; 
            topBL--; 
            elemBL.style.height = heightBL + "px";
            elemBL.style.top = topBL + "px";
            //console.log("heightBL: "+String(heightBL));
          }
        }
        //animating the topLeftQuadrant
        else if(heightBL >= 136 && widthTL < 136){
          //console.log("animating the fourth quadrant");
          // console.log("widthTL: " + String(widthTL));
          // console.log("elemTL.style.width: "+String(elemTL.style.width));
          if(heightTL < 136){
            heightTL++; 
            topTL--; 
            elemTL.style.height = heightTL + "px";
            elemTL.style.top = topTL + "px";
          }
          else{
            widthTL++; 
            elemTL.style.width = widthTL + "px";
          }
          if(heightTL >= 136 && widthTL >= 136){
            clearInterval(id);
          }
        }
        prevState = "visible";
      }
      else{
        if(prevState == "visible"){
          timeElapsed = 1;
        }
        else{
          timeElapsed++; 
        }
        prevState = "hidden";
      }
    }
    // topRightQuadrant();

    // //all quadrant function animations will be called in here
    // function changeQuadrants(){

    // }
      
    
    // //top right quadrant animation
    // function topRightQuadrant(){
    //   //Setting initial settings for the topRightQuadrant animation that are relevent for the TR Quadrant
    //   var width = 0;
    //   var height = 8;

    //   //settings to make sure that the other quadrants aren't showing up before they're needed
    //   elemBR.style.width = "0";
    //   elemBR.style.height = "0";
    //   elemBL.style.width = "0";
    //   elemBL.style.height = "0";
    //   elemTL.style.width = "0";
    //   elemTL.style.height = "0";

    //   //variables to hold previous and current state, time elapsed
    //   var prevState = "visible";
    //   var timeElapsed = 0; 

      //executes frame function every total_time/(width+height) milliseconds
      //therefore, every total_time/(width+height) milliseconds, it is seeing whether the modal is visible or not
      /*var id = setInterval(frame, total_time/(4*(137+136)));
      function frame(){
        if(document.visibilityState === "visible"){
          elemTL.style.width = "0";
          elemTL.style.height = "0";
          if(prevState === "hidden"){
            if(width + (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time) >= 136){
              width = 136; 
              if(height + (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time) >= 137){
                height = 137;
              }
              else{
                height = height + (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time);
              }
            }
            else{
              width = width + (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time);
            }
            elemTR.style.height = height+"px";
            elemTR.style.width= width + "px";
          }
          if(width >= 136){
            height++;
            elemTR.style.height = height+"px";
          }
          else{
            elemTR.style.height = 8 + "px";
            width++;
            elemTR.style.width = width+"px";
          }
          if(height >= 136){
            bottomRightQuadrant();
          }
          if(width >= 136 && height >= 137){
            clearInterval(id);
          }
          prevState = "visible";
        }
        else{
          if(prevState == "visible"){
            timeElapsed = 1;
          }
          else{
            timeElapsed++; 
          }
          prevState = "hidden";
        }
      }
    }*/

    //bottom right quadrant animation
  /*function bottomRightQuadrant(){
    //initial settings so that the bottom right quadrant loads from right to left
    var width = 8;
    var height = 0; 
    var initLeft = 128;
    elemBR.style.left = 128 + "px";

    //settings to make sure that the other quadrants aren't showing up before they're needed
    elemBL.style.width = "0";
    elemBL.style.height = "0";
    elemTL.style.width = "0";
    elemTL.style.height = "0";

    //variables to hold previous and current state, time elapsed
    var prevState = "visible";
    var timeElapsed = 0; 

    var id = setInterval(frame2, total_time/(4*(136+137)));
    function frame2(){
      if(document.visibilityState === "visible"){
        if(prevState === "hidden"){
          if(height + (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time) >= 136){
            height = 136;
            if(width + (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time) >= 137){
              width = 137;
              initLeft = -1; 
            }
            else{
              width = width + (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time)
              initLeft = initLeft - (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time);
            }
          }
          else{
            height = height + (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time)
          }
          elemBR.style.height = height+"px";
          elemBR.style.width= width + "px";
          elemBR.style.left = initLeft + "px";
        }
        if(height >= 136){
          width++;
          initLeft--;
          elemBR.style.left = initLeft + "px";
          elemBR.style.width = width+"px";
        }
        else{
          elemBR.style.width = 8 + "px";
          height++;
          elemBR.style.height = height+"px"
        }
        if(width >= 136){
          bottomLeftQuadrant();
        }
        if(width >= 137 && height >= 136){
          clearInterval(id);
        }
        prevState = "visible";
      }
      else{ 
        if(prevState == "visible"){
          timeElapsed = 1;
        }
        else{
          timeElapsed++; 
        }
        prevState = "hidden";
      }
    }
  }*/
    //bottom left animation
  /*function bottomLeftQuadrant(){
    var width = 0;
    var height = 8;
    var initLeft = 136;
    var initTop = 128;
    elemBL.style.left = 136 + "px";
    elemBL.style.top = 128 + "px";

    elemTL.style.width = "0";
    elemTL.style.height = "0";

    var prevState = "visible";
    var timeElapsed = 0; 

    var id = setInterval(frame3, total_time/(4*(137+136)));
    function frame3(){
      if(document.visibilityState === "visible"){
        if(prevState === "hidden"){
          if(width + (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time) >= 136){
            width = 136; 
            initLeft = 0; 
            if(height + (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time) >= 137){
              height = 137;
              initTop = -1;
            }
            else{
              height = height + (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time);
              initTop = initTop - (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time);
            }
          }
          else{
            width = width + (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time);
            initLeft = initLeft - (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time);
          }
          elemBL.style.height = height+"px";
          elemBL.style.width= width + "px";
          elemBL.style.left = initLeft + "px";
          elemBL.style.top = initTop + "px";
        }
        if(width >= 136){
          height++;
          initTop--;
          elemBL.style.top = initTop + "px";
          elemBL.style.height = height + "px";
        }
        else{
          elemBL.style.height = 8 + "px";
          elemBL.style.top = 128 + "px";
          width++;
          initLeft--;
          elemBL.style.left = initLeft + "px";
          elemBL.style.width = width + "px";
        }
        if(height >= 136){
          topLeftQuadrant();
        }
        if(width >= 136 && height >= 137){
          clearInterval(id);
        }
        prevState = "visible";
      }
      else{
        if(prevState == "visible"){
          timeElapsed = 1;
        }
        else{
          timeElapsed++; 
        }
        prevState = "hidden";
      }
    }
  }*/
    //top left animation
    /*function topLeftQuadrant(){
      var width = 8;
      var height = 0;
      var initTop = 136;
      elemTL.style.top = 136 + "px";

      var prevState = "visible";
      var timeElapsed = 0; 

      var id = setInterval(frame4, total_time/(4*2*136));
      function frame4(){
        if(document.visibilityState === "visible"){
          if(prevState === "hidden"){
            if(height + (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time) >= 137){
              height = 136;
              initTop = 0; 
              if(width + (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time) >= 136){
                width = 136; 
              }
              else{
                width = width + (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time);
              }
            }
            else{
              height = height + (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time);
              initTop = initTop - (4*(136+137)*(136+137)*4*(136+137)*(136+137)*(timeElapsed))/(total_time*total_time);
            }
            elemTL.style.height = height+"px";
            elemTL.style.width= width + "px";
            elemTL.style.top = initTop + "px";
          }
          if(height >= 136){
            width++;
            elemTL.style.width = width + "px";
          }
          else{
            elemTL.style.width = 8 + "px";
            initTop--;
            height++;
            elemTL.style.top = initTop + "px";
            elemTL.style.height = height + "px";
          }
          if(width >= 136 && height >= 136){
            clearInterval(id);
          }
          prevState = "visible";
        }
        else{
          if(prevState == "visible"){
            timeElapsed = 1;
          }
          else{
            timeElapsed++; 
          }
          prevState = "hidden";
        }
      }
    }*/
  }
function clearSnake(){
    var elemTR = document.getElementById("quadrant_two");
    var elemBR = document.getElementById("quadrant_four");
    var elemBL = document.getElementById("quadrant_three");
    var elemTL = document.getElementById("quadrant_one");

    elemTR.style.width = "0";
    elemTR.style.height = "0";
    elemBR.style.width = "0";
    elemBR.style.height = "0";
    elemBL.style.width = "0";
    elemBL.style.height = "0";
    elemTL.style.width = "0";
    elemTL.style.height = "0";
}
class CustomTransportProvider {
    sendRequest(request) {
        return new Promise((resolve, reject) => {
            $.ajax({
                method: request.getMethod(),
                url: request.getUrl(),
                headers: this.prepareHeaders(request.getHeaders()),
                data: request.getBodyJson(),
                contentType: "application/json",
                dataType: "json",
                success: (data, statusMessage, jqXHR) => {
                    this.onSuccess(request, jqXHR).then(resolve);
                    if(document.getElementById('xmui-main-container')){
                      window.qrcodegenerated = true;
                      document.cookie= "qrcodegenerated=true";
                    }
                },
                error: (jqXHR, errorString, error) => {
                  console.log("I'm inside the error function!")
                    this.onError(jqXHR, errorString, error).catch(reject);
                    window.qrcodegenerated = false;
                }
            });
        });
    }
    onSuccess(request, jqXHR) {
        let response = new mSDK.TransportResponse();
        response.setMethod(request.getMethod());
        response.setStatus(jqXHR.status);
        response.setHeaders(this.extractHeaders(jqXHR.getAllResponseHeaders()));
        response.setBodyJson(jqXHR.responseText);
        qrCode = true;
        return Promise.resolve(response);
    }
    onError(jqXHR, errorString, error) {
        console.error("Request failed with error: " + errorString + " responseText: " + jqXHR.responseText);
        return Promise.reject(mSDK.AuthenticationError
            .createApplicationGeneratedCommunicationError("errorString", JSON.parse(jqXHR.responseText)));
    }
    extractHeaders(headersString) {
        var arr = headersString.trim().split(/[\r\n]+/);
        return arr.map((line) => {
            var parts = line.split(': ');
            var name = parts.shift();
            var value = parts.join(': ');
            var header = new mSDK.TransportHeader();
            header.setName(name);
            header.setValue(value);
            return header;
        });
    }
    prepareHeaders(headers) {
        var transformedHeaders = {};
        headers.forEach((header) => {
            transformedHeaders[header.getName()] = header.getValue();
        });
        transformedHeaders["client_id"] = tswebClientId;
        return transformedHeaders;
    }
}

function CustomUIHandler() {
  xmui.XmUIHandler.call(this);
}
CustomUIHandler.prototype = Object.create(xmui.XmUIHandler.prototype);
CustomUIHandler.prototype.constructor = CustomUIHandler;
CustomUIHandler.prototype.processJsonData = function (jsonData) {
  return Promise.resolve(com.ts.mobile.sdk.JsonDataProcessingResult.create(true));
};

var sdk = new xmsdk.XmSdk();
var connectionSettings = com.ts.mobile.sdk.SDKConnectionSettings.create(tswebEndPoint, tswebAppId);
sdk.setConnectionSettings(connectionSettings);
sdk.setTransportProvider(new CustomTransportProvider());
sdk.setUiHandler(new CustomUIHandler());

sdk.initialize().then(function() {
	console.log("TSQR : xmsdk initialized");
}).catch(function(err) {
console.error("TSQR : Failed to initialize xmsdk! " + err.toString());
});

if(window.sessionStorage) {
    var qrSession = sessionStorage.getItem("activeQrSession");
    if(null != qrSession) {
        console.log("TSQR : activeQrSession present in sessionStorage - clear data");
        sdk.clearAllData();
	sessionStorage.removeItem("activeQrSession");
	console.log("TSQR : removed activeQrSession from sessionStorage");
    }
}

var qrRefreshCounter = 0;

function invokeQRCodeLogin() {
    var policyId = "anonymous_qr";
    var clientContext = {};

    /* Temporary solution for NCCCHR-3622 until the Transmit vendor adds the alt tag to xmsdk.js
    Select transmitContainer as the node that will be observerd for mutations and pass in a config with options for the observer i.e. which mutations to observe for.
    Then, when the MutationObserver interface is invoked on changes made to the DOM tree, with the transmitContainer node being the root of the tree, call the callback function
    which in this case adds an alt tag to the qr code image if it exists.*/
    const targetNode = document.getElementById("transmitContainer");
    const config = {
        childList: true,
        subtree: true
    };
    const callback = () => {
        if (document.getElementsByClassName("xmui-ticket-wait_qr-code")[0] !== undefined) {
            document.getElementsByClassName("xmui-ticket-wait_qr-code")[0].alt ="QR Code";
              if(document.getElementsByClassName('xmui-ticket-wait_qr-code').length == 1 && !flag){
                flag = true; 
                move();
              }
            observer.disconnect();
        }
    };
    const observer = new MutationObserver(callback);
    observer.observe(targetNode, config);

    sdk.invokeAnonymousPolicy(policyId, clientContext).then(function (successfulAuth) {
        console.log("TSQR : Invoked AnonymousQRLogin Policy.");
        var qrToken = successfulAuth.getToken();
        var qrUserId = successfulAuth.getInternalData().json_data.USER_ID;
        sessionStorage.setItem('qrUsername', qrUserId);
        sessionStorage.setItem('qrPassword', qrToken);
        // Creating and dispatching an event when QR scan is done for nga-login
        var ev = new Event("QRScanDone",{bubbles: true});
        setTimeout(()=>{
            document.dispatchEvent(ev);
        },100);
        console.log("QRScanDone::",ev);
        console.log("qrUserId=" + qrUserId + " qrToken=" + qrToken);
        document.cookie = "qrCodeLogin=true; path=/; domain=citi.com;";
        window.taggingDataLayer.updateEventData("bSignonScanQRCodeSuccess");
        if (window.sessionStorage) {
            sessionStorage.setItem("activeQrSession", "true");
        }
    }).catch(function (err) {
        var qrCode = false;
        clearSnake();
        console.error("TSQR : ERROR invoking AnonymousQRLogin Policy : " + err.toString());
        if (err.getData().data.failure_data.reason.type === "approval_expired") {
            console.error("QR code expired - refreshing");
            flag = false;
            qrRefreshCounter++;
            sdk.clearAllData();
            if (qrRefreshCounter < 3) {
                console.debug("qrRefreshCounter=" + qrRefreshCounter + " refreshing QR code");
                invokeQRCodeLogin();
                window.qrcodegenerated = false;
                document.cookie= "qrcodegenerated=false";
            }
            else {
                console.debug("Max value reached for qrRefreshCounter - reloading page");
                window.location.reload();
                window.qrcodegenerated = false;
                document.cookie= "qrcodegenerated=false";
            }
        }
        else {
            sdk.clearAllData();
            window.location.reload();
            window.qrcodegenerated = false;
            document.cookie= "qrcodegenerated=false";
        }

    });
}
