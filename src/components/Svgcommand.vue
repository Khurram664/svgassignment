<template>
    <div>
        <p>Enter Command in following text area:</p>
        <br/>
        <textarea name="commandarea" class="textareaclass" id="commandarea" cols="30" rows="10"></textarea>
        <br/>

        <p id ="p_error" style="color:red"></p>
        <br/>
        <input type="button"  @click="draw"  class="btn btn-success command-button" value="Draw" >
    
        <br/>
        <br/>
        <input type="button"  @click="clear"  class="btn btn-primary command-button" value="Clear" >

         <br>
        <svg id="svg_result" width="80%" height="900px" style="margin-left:5px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
            
        </svg>
    </div>
</template>

<script>

export default {
    name: "Svgcommand",
     methods : {

         clear(){

            let svg1 = document.getElementById("svg_result");
            svg1.innerHTML = '';
            
            let p_error = document.getElementById("p_error");
            p_error.innerHTML = '';

            let textArea = document.getElementById("commandarea");
            textArea.value = "";


         },
        draw(){
             
            
            let svg1 = document.getElementById("svg_result");
            svg1.innerHTML = '';

            let textArea = document.getElementById("commandarea");
            let arrayOfLines = textArea.value.split("\n"); // arrayOfLines is array where every element is string of one line

            let p_error = document.getElementById("p_error");
            p_error.innerHTML = '';
    
            arrayOfLines.forEach((command,index) => {
                console.log(command);
                let line = index + 1;
                if(command === ""){
                    p_error.innerHTML = "Please enter a command e.g c 10 15 30";
                }
                else{
                    
                    let command_array = command.split(" ");

                    if(command_array[0] != 'c' && command_array[0] != 'r' && command_array[0] != 'p')
                         p_error.innerHTML = `Please enter a valid command starting with c or r  or p on line ${line}`;

                    else if(command_array[0] == 'c'){
                        if(command_array.length == 4 && !isNaN(command_array[1]) && !isNaN(command_array[2]) && !isNaN(command_array[3]) )
                             this.drawCircle(svg1,command_array[1],command_array[2],command_array[3]);
                        else
                             p_error.innerHTML = `Please enter a proper command e.g c 10 15 30 on line ${line}`;
                    }

                    else if(command_array[0] == 'r'){
                        if(command_array.length == 5 && !isNaN(command_array[1]) && !isNaN(command_array[2]) && !isNaN(command_array[3]) && !isNaN(command_array[4]) )
                             this.drawReactangle(svg1,command_array[1],command_array[2],command_array[3],command_array[4]);
                        else
                             p_error.innerHTML = `Please enter a proper command e.g r 10 15 30 50 ${line}`;
                    }

                    else if(command_array[0] == 'p'){

                        let isFormat = true;

                        console.log(command_array[0]);
                        console.log(command_array[1]);

                        command_array.forEach((item,index) => {
                            //check if comma is there
                            if(index > 0){
                                if(item.indexOf(',') > -1){
                                    let items_Array = item.split(",");
                                    if(!isNaN(items_Array[0]) && !isNaN(items_Array[1])){
                                        console.log(item);
                                        
                                    }
                                    else{
                                        
                                        isFormat = false;
                                    }
                                }
                                else{
                                   
                                    isFormat = false;
                                }    
                            }
                        })

                        if(isFormat == true ){
                            delete command_array[0];
                            this.drawPolygon(svg1,command_array.join(" "));
                        }
                        else{
                            p_error.innerHTML = `Please enter a proper command e.g p 200,10 250,190 160,210 on line ${line}`;
                        }
                    }

                }
            })

        },
        getRandomColor() {
            var letters = '0123456789ABCDEF';
            var color = '#';
            for (var i = 0; i < 6; i++) {
                color += letters[Math.floor(Math.random() * 16)];
            }
            return color;
        },
        drawCircle(ele,cx,cy,r){

            var circles = document.createElementNS("http://www.w3.org/2000/svg", "circle");
            let color =  this.getRandomColor();
            circles.setAttribute("cx",cx);
            circles.setAttribute("cy",cy);
            circles.setAttribute("r",r);
            circles.setAttribute("fill",color);
            ele.appendChild(circles);

        },
        drawReactangle(ele,x,y,height,width){

            let color =  this.getRandomColor();

            var rectangle = document.createElementNS("http://www.w3.org/2000/svg", "rect");
            rectangle.setAttribute("x",x);
            rectangle.setAttribute("y",y);
            rectangle.setAttribute("height",height);
            rectangle.setAttribute("width",width);
            rectangle.setAttribute("fill",color);
            ele.appendChild(rectangle);
        },
        drawPolygon(ele,points){
        

            console.log("in daw polygon");
            console.log(points);
            let color =  this.getRandomColor();

            var polygon = document.createElementNS("http://www.w3.org/2000/svg", "polygon");
            polygon.setAttribute("points",points);
            polygon.setAttribute("fill",color);
            ele.appendChild(polygon);
        }

     },
    created(){
       console.log("in svg command");
    }        
}
</script>

<style scoped>

 .textareaclass {
    flex: 10;
    padding: 5px;
  }
  .command-button {
    flex: 2;
  }
  .btn {
    display: inline-block;
    margin-bottom: 0;
    font-weight: 400;
    text-align: center;
    white-space: nowrap;
    vertical-align: middle;
    -ms-touch-action: manipulation;
    touch-action: manipulation;
    cursor: pointer;
    background-image: none;
    border: 1px solid transparent;
    padding: 6px 12px;
    font-size: 14px;
    line-height: 1.42857143;
    border-radius: 4px;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}
.btn-success {
    color: #fff;
    background-color: #5cb85c;
    border-color: #4cae4c;
}
.btn-primary {
    color: #fff;
    background-color: #337ab7;
    border-color: #2e6da4;
}
</style>