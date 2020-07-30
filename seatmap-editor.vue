<template>
    <div>
        <div id="root" class="bnePEP">
            <div class="container">                
                <div class="d-flex align-items-center">
                    <h2 class="title" style="margin-right: 5px">Seat map editor</h2> 
                    <el-button @click="loadFile">Import svg</el-button>
                    <el-button @click="addSeatRows">Seat Rows</el-button>
                    <el-dropdown trigger="click" placement="top-start">
                        <span class="el-dropdown-link">Tabe</span>
                        <el-dropdown-menu slot="dropdown">
                            <el-dropdown-item>
                                <span class="d-block" @click="addRectangleTable">Rect table</span>
                            </el-dropdown-item>  
                            <el-dropdown-item>
                                <span class="d-block" @click="testAddAny">Circle table</span>
                            </el-dropdown-item>   
                        </el-dropdown-menu>
                    </el-dropdown>        
                    <el-dropdown trigger="click" placement="top-start">
                        <span class="el-dropdown-link">Shape</span>
                        <el-dropdown-menu slot="dropdown">
                            <el-dropdown-item>
                                <span class="d-block" @click="addZoneShape">Zone</span>
                            </el-dropdown-item>  
                            <el-dropdown-item>
                                <span class="d-block" @click="addCircleShape">Circle</span>
                            </el-dropdown-item>
                            <el-dropdown-item>
                                <span class="d-block" @click="addRectShape">Rectangle</span>
                            </el-dropdown-item>                            
                            <el-dropdown-item> 
                                <span class="d-block" @click="addSectorShape">Annular sector</span>  
                            </el-dropdown-item>                      
                        </el-dropdown-menu> 
                    </el-dropdown>                                      
                </div>             
            </div>
            <div class="bnePEP" style="flex-direction: row;">
                <div id ="editorArea" class="gRightEditor">
                    <div id="seatmap" class="svg-container">
                        <svg id="diagrams-obj" xmlns:svg="http://www.w3.org/2000/svg"
                            xmlns="http://www.w3.org/2000/svg"
                            version="1.1"
                            width="800"
                            height="400"
                            viewBox="0 0 800 400"> 
                            <!-- <pattern id="pattern-checkers" x="0" y="0" width="10" height="10" patternUnits="userSpaceOnUse">
                                <rect class="checker" x="0" width="5" height="5" y="0"></rect>
                                <rect class="checker" x="5" width="10" height="10" y="10"></rect>
                            </pattern>                                    -->
                            <!-- <path d="M 100 350 q 150 -200 300 0" stroke="blue"
                            stroke-width="2" fill="none" />                             -->
                            <g id="backface"> 
                                <!-- onload="makeDraggable(evt)"                    -->
                            <!-- <circle r="10" cx="250" cy="200" style="fill:yellow;"/>  -->
                            <!-- <path id="test" d="M 48 140 L 48 29 L 235 29 L 235 140 Z" stroke="black" stroke-width="2" opacity="1" 
                            fill="url(#pattern-checkers)">
                            </path>   -->
                            </g>
                        </svg>                         
                    </div>
                </div> 
                <div class="gLeftInfor">
                    <div id="selectionPanel" class="tilteGrItem">SELECTION</div>
                    <div class="gOpoHy">
                        <div class="rightItems">
                            <div style="display: flex;">
                                <div width="95px" class="sc-fzoiQi kCdYf">Selection</div>
                                <div id="zoneId" class="sc-fzoCCn lewZOv">{{selectObjKind}}</div>
                            </div>
                        </div> 
                        <div class="rightItems">
                            <div style="display: flex;">
                                <div width="95px" class="sc-fzoiQi kCdYf">Text</div>
                                <input maxlength="80" class="fcJrYL" v-model="zoneSelection.name" v-on:change="addTextToShape">
                            </div>
                        </div> 
                        <div class="rightItems"> 
                            <div style="display: flex;">
                                <div width="95px" class="sc-fzoiQi kCdYf">Font size</div>
                                <input type="number" min="1" max="6" v-model="zoneSelection.fontSize" v-on:change="changeFontSize"
                                class="numbox"/>  
                            </div>                            
                        </div> 
                        <div class="rightItems"> 
                            <div style="display: flex;">
                                <div width="95px" class="sc-fzoiQi kCdYf">Color</div>
                                <el-color-picker v-model="colorfill" v-on:change="changeColorShape"></el-color-picker>
                            </div>
                        </div>                                               
                    </div>                                            
                    <div id="div-SeatRows" class="gOpoHy" style="display: none">
                        <div class="rightItems">
                            <div style="display: flex;">
                                <div width="95px" class="sc-fzoiQi kCdYf">Seats</div>
                                <div id="rowCol" class="sc-fzoCCn lewZOv">0 seats</div>
                            </div>
                        </div> 
                        <div class="rightItems">
                            <div style="display: flex;">
                                <div width="95px" class="sc-fzoiQi kCdYf">Row Labels</div>
                                <div class="sc-fzoYHE gwozhV">
                                    <button class="selected" @click="addRowNumberLabel">123</button>
                                    <button @click="addRowAlphabetLabel">ABC</button>
                                </div>
                            </div>
                        </div> 
                        <div class="rightItems">
                            <div style="display: flex;">
                                <div width="95px" class="sc-fzoiQi kCdYf">Seats to zone</div>
                                <div class="sc-fzoYHE gwozhV">
                                    <button @click="clearZone">Clear</button>
                                    <button class="selected" @click="mountToZone">Mount</button>
                                </div>
                            </div>
                        </div>                                                
                    </div> 
                    <div class="tilteGrItem">TRANSFORM</div> 
                    <div class="gOpoHy">
                        <div id="div-trans-seatrows" class="rightItems" style="display: none;">
                            <div style="display: flex;">
                                <div width="95px" class="sc-fzoiQi kCdYf">Rotate</div>
                                <div style="display: flex">
                                    <input type="range" min="-180" max="180" v-model="rotate" v-on:change="rotateShapeObject"
                                    style="width: 100px"/>
                                    <input type="number" min="-180" max="180" v-model="rotate" v-on:change="rotateShapeObject"
                                    class="numbox"/>
                                </div>    
                            </div>                                                        
                            <div style="display: flex;">
                                <div width="95px" class="sc-fzoiQi kCdYf">Stretch</div>
                                <div style="display: flex">
                                    <input disabled type="range" min="1" max="60" v-model="stretchRow" style="width: 100px"/>
                                    <input disabled type="number" min="1" max="60" v-model="stretchRow" class="numbox"/>
                                </div>    
                            </div>  
                            <div style="display: flex;">
                                <div width="95px" class="sc-fzoiQi kCdYf">Curve</div>
                                <div style="display: flex">
                                    <input type="range" min="-50" max="50" v-model="curve" v-on:change="makeCurveSeatRows"
                                    style="width: 100px"/>
                                    <input type="number" min="-50" max="50" v-model="curve" v-on:change="makeCurveSeatRows"
                                    class="numbox"/>
                                </div>        
                            </div>                                                                                                                                                      
                        </div>    
                        <div id="div-trans-size" class="rightItems" style="display: none;">                              
                            <div style="display: flex">
                                <div width="50px" class="sc-fzoiQi kCdYf">Dimensions</div>
                                <div style="display: flex">
                                    <input type="number" min="20" v-model="d_width" v-on:change="changeShapeSize" 
                                    class="numbox" style="margin-right: 5px; width: 70px"/>
                                    <input type="number" min="20" v-model="d_height" v-on:change="changeShapeSize"
                                    class="numbox" style="width: 70px"/> 
                                </div> 
                            </div>
                        </div>                               
                        <div id="div-angle" class="rightItems" style="display: none;">
                            <div style="display: flex">
                                <div width="50px" class="sc-fzoiQi kCdYf">Angle</div>
                                <div style="display: flex">
                                    <input type="range" min="30" max="120" v-model="angle" v-on:change="changeAngleShape" 
                                    style="width: 100px"/>
                                    <input type="number" min="30" max="130" v-model="angle" v-on:change="changeAngleShape"
                                    class="numbox"/> 
                                </div> 
                            </div> 
                        </div>
                        <div id="div-rotate" class="rightItems" style="display: none;"> 
                            <div style="display: flex;">
                                <div width="95px" class="sc-fzoiQi kCdYf">Rotate</div>
                                <div style="display: flex">
                                    <input type="range" min="-180" max="180" v-model="rotate" v-on:change="rotateShapeObject"
                                    style="width: 100px"/>
                                    <input type="number" min="-180" max="180" v-model="rotate" v-on:change="rotateShapeObject"
                                    class="numbox"/> 
                                </div>    
                            </div>                             
                        </div>                                                          
                        <div id="div-trans-circle">
                            <div style="display: flex">
                                <div width="50px" class="sc-fzoiQi kCdYf">Dimensions</div>
                                <div style="display: flex">
                                    <input type="number" min="20" v-model="d_width" v-on:change="changeShapeSize" 
                                    class="numbox" style="width: 70px"/>
                                </div> 
                            </div>                             
                        </div>
                        <div id = "div-trans-zone" class="rightItems" style="display: block"> 
                            <button id="transformBtn" class="lnIXeA Btn-fullWidth" @click="createSimpleSeatMap">Creat simple seat map</button> 
                            <div style="display: flex;">
                                <div width="95px" class="sc-fzoiQi kCdYf">Rows</div>
                                <input type="number" min="8" max="14" v-model="row" v-on:change="drawSimpleSeatMap" class="numbox"/>                                   
                                 
                            </div>  
                            <div style="display: flex">
                                <div width="95px" class="sc-fzoiQi kCdYf">Seats a row</div>
                                <input type="number" min="8" max="20" v-model="col" v-on:change="drawSimpleSeatMap" class="numbox"/>
                            </div>                           
                        </div>                          
                    </div>   
                    <div class="tilteGrItem">ACTIONS</div> 
                    <div class="gOpoHy">
                        <div class="rightItems">
                            <div style="display: flex;">
                                <div width="95px" class="sc-fzoiQi kCdYf">Actions</div>  
                                <div style="display: flex;">
                                    <button class="lnIXeA" v-bind:disabled="denyRemove" @click="removeElement">Delete</button>
                                    <button class="lnIXeA" disabled>Clone</button>
                                </div>                                      
                            </div>    
                        </div>                                
                        <div class="rightItems">
                            <div style="display: flex;">
                                <div width="95px" class="sc-fzoiQi kCdYf">Status</div>
                                <span id="statusMessage" style="color:#1d6284" class="lewZOv">draft</span>
                            </div>    
                        </div> 
                    </div>                     
                </div>          
            </div> 
        </div>         
    </div> 
</template>
<script>
// SVGElement.prototype.getTransformToElement = SVGElement.prototype.getTransformToElement || function(elem) {
//     return elem.getScreenCTM().inverse().multiply(this.getScreenCTM());
// }
const NAV_MAP = {
		187: { dir:  1, act: 'zoom', name: 'in' } /* + */, 
        61: { dir:  1, act: 'zoom', name: 'in' } /* + WTF, FF? */, 
        189: { dir: -1, act: 'zoom', name: 'out' } /* - */, 
        173: { dir: -1, act: 'zoom', name: 'out' } /* - WTF, FF? */, 
        37: { dir: -1, act: 'move', name: 'left', axis: 0 } /* ⇦ */, 
        38: { dir: -1, act: 'move', name: 'up', axis: 1 } /* ⇧ */, 
        39: { dir:  1, act: 'move', name: 'right', axis: 0 } /* ⇨ */, 
        40: { dir:  1, act: 'move', name: 'down', axis: 1 } 
    }

export default {
    data() {
        return {
            index: 0,
            row: 0,
            col: 0,
            radius: 5,
            rotate: 0,
            stretchRow: 3,
            stretchCol: 3, 
            curve: 0,
            activeMove: null,
            actTransform: 0,
            eVB_width: 0,
            eVB_height: 0,
            d_width: 0,
            d_height: 0,
            zoneSelection: {
                name: '',
                id: '',
                fontSize: 1
            },
            // seatRows: {
            //     frameId:'',
            //     stretchRow: 3,
            //     stretchCol: 3,    
            //     seats: []
            // },
            mapData:[],
            angle: 0,
            selectObjKind: 0,
            selectStyle: null, 
            selectionId: null,
            x1: 0,
            y1: 0, 
            colorfill: null,
            zoomMode: false,
            drawingMode: false,                     
            denyRemove: true
        }
    },
    methods: {
        testAddAny(){
            alert('this version not support!')
            // var svg = document.getElementsByTagName('svg')[0] 
            // function getQuadraticBezierXYatPercent(startPt,controlPt,endPt,percent) {
            //     var x = Math.pow(1-percent,2) * startPt.x + 2 * (1-percent) * percent * controlPt.x + Math.pow(percent,2) * endPt.x; 
            //     var y = Math.pow(1-percent,2) * startPt.y + 2 * (1-percent) * percent * controlPt.y + Math.pow(percent,2) * endPt.y; 
            //     return( {x:x,y:y} );
            // }   
            // var point4 = getQuadraticBezierXYatPercent({x:100,y:350},{x:250,y:150},{x:400,y:350},1)
            // var point3 = getQuadraticBezierXYatPercent({x:100,y:350},{x:250,y:150},{x:400,y:350},0.75)
            // var point1 = getQuadraticBezierXYatPercent({x:100,y:350},{x:250,y:150},{x:400,y:350},0.5) 
            // var point2 = getQuadraticBezierXYatPercent({x:100,y:350},{x:250,y:150},{x:400,y:350},0.25)
            // var point0 = getQuadraticBezierXYatPercent({x:100,y:350},{x:250,y:150},{x:400,y:350},0)
            // insertCircle(point0.x, point0.y, "#a0a")
            // insertCircle(point1.x, point1.y, "#a0a")
            // insertCircle(point2.x, point2.y, "#a0a")
            // insertCircle(point3.x, point3.y, "#a0a") 
            // insertCircle(point4.x, point4.y, "#a0a")     
            // function insertCircle(posX, posY, color)
            // {
            //     let shape = document.createElementNS("http://www.w3.org/2000/svg", 'circle');
            //     shape.setAttributeNS(null, 'cx', posX);
            //     shape.setAttributeNS(null, 'cy', posY);
            //     shape.setAttributeNS(null, 'r', 10);
            //     shape.setAttributeNS(null, 'fill', color);
            //     svg.appendChild(shape);
            // }

            // svg.addEventListener('click', event =>
            // {
            //     let bound = svg.getBoundingClientRect();
            //     let style = getComputedStyle(svg);
                
            //     let paddingLeft = parseFloat(style['padding-left'].replace('px', ''));
            //     let paddingTop = parseFloat(style['padding-top'].replace('px', ''));

            //     let x = event.clientX - bound.left - svg.clientLeft - paddingLeft;
            //     let y = event.clientY - bound.top - svg.clientTop - paddingTop;
                
            //     if(event.x === undefined)
            //     {
            //         x -= parseFloat(style['border-left-width'].replace('px', ''));
            //         y -= parseFloat(style['border-top-width'].replace('px', ''));
            //     }
                
            //     let width = svg.width.baseVal.value;
            //     let height = svg.height.baseVal.value;
                
            //     if(x < 0 || y < 0 || x >= width || y >= height)
            //     {
            //         return;
            //     }                             
            //     // insertRect(x, y, "#a0a");
            // })                      
        },
        loadFile() {         
            var dataSVG ="";
            $.ajax({
                type: "GET",
                url: "/seatmap-svg/stadium_02.svg",
                dataType:"text",
                async: false,
                success: function(response){
                    var svgTag = response.indexOf("svg"); 
                    var firstText = response.slice(0, svgTag+3)
                    var text = firstText + " id='diagrams-obj' onload='makeDraggable(evt)'"
                    //if request if made successfully then the response represent the data                    
                    // $( "#result" ).empty().append( response );
                    dataSVG = text + response.slice(svgTag+3, response.length); 
                }
            });
            console.log(dataSVG)
            var seatmap_div = document.getElementById('seatmap');            
            seatmap_div.innerHTML = dataSVG; 
            var svg = document.getElementById('diagrams-obj');
            svg.setAttribute("preserveAspectRatio", "xMinYMin meet");
            var css = 'path:hover{ opacity: 0.5 }';
            var style = document.createElement('style');
            if (style.styleSheet) {
                style.styleSheet.cssText = css;
            } else {
                style.appendChild(document.createTextNode(css));
            } 
            svg.appendChild(style) 
            var backface = document.createElementNS('http://www.w3.org/2000/svg', 'g');
            backface.id = "backface"
            svg.appendChild(backface)
            var positionInfo = seatmap_div.getBoundingClientRect()     
            this.eVB_width = positionInfo.width.toFixed(0)
            this.eVB_height= positionInfo.height.toFixed(0) 
            svg.setAttribute('width',  this.eVB_width) 
            svg.setAttribute('height', this.eVB_height)    
            svg.setAttribute("viewBox", [0, 0, this.eVB_width, this.eVB_height].join(' '))
            this.svgZoomInOutMove()
            this.mouseOverOutEffect()                               
        },
        listenerKeyZoomAndMove() {
            var _SVG = document.querySelector('svg')
            var VB = _SVG.getAttribute('viewBox').split(' ').map(c => +c)
            var DMAX = VB.slice(2)
            var WMIN = 8
            var NF = 16

            let rID = null
            var f = 0
            var nav = {}
            var tg = Array(4)

            function stopAni() {
                cancelAnimationFrame(rID);
                rID = null;  
            };
            
            function update() {
                let k = ++f/NF, j = 1 - k, cvb = VB.slice();
                
                if(nav.act === 'zoom') {		
                    for(let i = 0; i < 4; i++)
                        cvb[i] = j*VB[i] + k*tg[i]
                        
                }
                
                if(nav.act === 'move')	
                    cvb[nav.axis] = j*VB[nav.axis] + k*tg[nav.axis];
                //_SVG.setAttribute('viewBox', cvb.join(' '));
                document.getElementById('diagrams-obj').setAttribute('viewBox', cvb.join(' '));
                
                if(!(f%NF)) {
                    f = 0;
                    VB.splice(0, 4, ...cvb);
                    nav = {};
                    tg = Array(4);
                    stopAni();
                    return;
                }
            
                rID = requestAnimationFrame(update)
            };             

            addEventListener('keydown', e => { e.preventDefault() }, false);
            addEventListener('keypress', e => { e.preventDefault() }, false);

            addEventListener('keyup', e => {
                e.preventDefault();  
                if(!rID && e.keyCode in NAV_MAP) {
                    nav = NAV_MAP[e.keyCode];
                    
                    if(nav.act === 'zoom') {
                        if((nav.dir === -1 && VB[2] >= DMAX[0]) || 
                            (nav.dir ===  1 && VB[2] <= WMIN)) {
                            console.log(`cannot ${nav.act} ${nav.name} more`);
                            return
                        }
                        
                        for(let i = 0; i < 2; i++) {
                            tg[i + 2] = VB[i + 2]/Math.pow(2, nav.dir);
                            tg[i] = .5*(DMAX[i] - tg[i + 2]);
                        }                        
                    }
                    
                    else if(nav.act === 'move') {
                        if((nav.dir === -1 && VB[nav.axis] <= 0) || 
                            (nav.dir ===  1 && VB[nav.axis] >= DMAX[nav.axis] - VB[2 + nav.axis])) {
                            console.log(`at the edge, cannot go ${nav.name}`);
                            return
                        }
                        
                        tg[nav.axis] = VB[nav.axis] + .5*nav.dir*VB[2 + nav.axis];
                    }
                    
                    update()
                }
            }, false);
        },  
        mouseOverOutEffect() {
            let me = this
            var osx = 0
            var osy = 0
            var diagramsObj = document.getElementById('diagrams-obj')
            function getTextShape(selectionId) {
                var id = selectionId 
                id = id.replace('zone','')
                id = id.replace('rect','')
                id = id.replace('zone','')
                id = id.replace('circle','')  
                id = id.replace('asector','')
                var textDom = document.getElementById('text'+id)
                if (textDom) {   
                    me.zoneSelection.name = textDom.innerHTML
                } else {
                    me.zoneSelection.name = ''   
                }
            }  
            function getShapeData(selectionId) {
                let shapeData = me.mapData.filter((shape) => {
                    return shape.id === selectionId 
                }) 
                if (shapeData && shapeData.length > 0) { 
                    me.x1 = shapeData[0].x1
                    me.y1 = shapeData[0].y1
                    me.d_width = shapeData[0].width 
                    me.d_height= shapeData[0].height 
                    me.angle  = shapeData[0].angle    
                }                    
            }           
            diagramsObj.onmouseover = function (e) {
                var event = e || window.event;
                var target = event.target || event.srcElement;   
                if (target.id != 'diagrams-obj') {            
                    //target.style.cursor = "move" 
                    statusMessage.innerText = 'click and hold to move'                                      
                }
                // document.getElementById('display').innerHTML = target.previousSibling.tagName +
                //     " | " + target.tagName + " | " + (target.nextSibling ? target.nextSibling.tagName : "X");
                // target.style.border = "1px solid";
                // lastelem = target;
            }
            diagramsObj.onmouseout = function (e) {
                // var event = e || window.event;
                // var target = event.target || event.srcElement;
                //target.style.cursor = 'pointer'
                statusMessage.innerText = 'draft'         
            }
            // SVGElement.prototype.getTransformToElement = SVGElement.prototype.getTransformToElement || function(elem) {
            //     return elem.getScreenCTM().inverse().multiply(this.getScreenCTM());
            // }      
            diagramsObj.onclick = function (e) {
                if (me.actTransform == 1) {
                    me.actTransform = 0 
                    me.transformShape()       
                }  
                var event = e || window.event;
                var target = event.target || event.srcElement;  
                if (me.selectionId) {
                    var oldSelectedDom = document.getElementById(me.selectionId)
                    if (oldSelectedDom) {
                        oldSelectedDom.setAttributeNS(null, "stroke", "none")
                        // if (oldSelectedDom.tagName=='path' || target.tagName=='circle' || target.tagName=='rect') {
                        //     //oldSelectedDom.style.fill = me.selectStyle
                        //     oldSelectedDom.setAttributeNS(null, "stroke", "none") 
                        // // } else if (oldSelectedDom.tagName=='circle' && me.selectionId.indexOf('seat')>=0) {
                        // //     oldSelectedDom.setAttributeNS(null, "stroke", "none")    
                        // }
                    }
                }                            
                me.selectionId = target.id;               
                if (target.tagName == 'path' || target.tagName=='circle' || target.tagName=='rect') {
                    me.activeMove = true
                    me.colorfill = target.getAttributeNS(null,'fill')
                    //me.selectStyle = target.style.fill;
                    me.zoneSelection.id = target.id
                    me.denyRemove = false
                    //target.style.fill = "#1199ee" 
                    target.setAttributeNS(null, "stroke", "#1d6284") 
                    target.setAttributeNS(null, "stroke-width", 1) 
                    target.setAttributeNS(null, "stroke-opacity",1)  
                    if (target.id.indexOf('zone') >= 0) {
                        me.selectObjKind = 'zone' 
                        me.actTransform = 1  
                        me.transformShape()                      
                    } else if (target.id.indexOf('sRows') >= 0) {
                        me.selectObjKind = 'seatRows'                      
                    } else if (target.id.indexOf('asector') >= 0) {
                        me.selectObjKind = 'sector'  
                        getShapeData(target.id)                               
                    } else {
                        me.selectObjKind = target.tagName                         
                    }
                    selectionPanel.innerText='SELECTION'      
                    if (target.tagName=='rect') {
                        me.d_width = target.getAttribute('width')
                        me.d_height = target.getAttribute('height')   
                    } else if (target.tagName=='circle') {
                        me.d_width = target.getAttribute('r')
                    } 
                    zoneId.innerText = me.selectObjKind.toUpperCase()  
                    getTextShape(target.id)      
                // } else if (target.tagName=='circle' && me.selectionId.indexOf('seat')>=0) {
                //     me.activeMove = true
                //     target.setAttributeNS(null, "stroke", "#1d6284") 
                //     target.setAttributeNS(null, "stroke-width", 1)  
                //     me.denyRemove = false
                } else {
                    me.denyRemove = true
                    selectionPanel.innerText='SEAT MAP'
                    me.selectObjKind = 'map'                    
                }
                if (me.activeMove) {                      
                    me.makeDraggable(event)
                }
                me.activeDeactiveComponent()  
            },
            //zoom svg element when double click
            diagramsObj.ondblclick = function (e) {
                // var event = e || window.event;
                // var target = event.target || event.srcElement; 
                // var id =  target.getAttribute("id")
                // //get main SVG object and its current viewBox for zoom 
                // var svg = document.querySelector("svg");                
                // var vbox = svg.getAttribute('viewBox').split(' ').map(c => +c)              
                // // the current center of the viewBox
                // var cx=vbox[0]+vbox[2]/2;
                // var cy=vbox[1]+vbox[3]/2;
                // // element is the element I want to zoom to
                // var element = document.querySelector('#'+id)                
                // var bbox = element.getBBox()
                // var matrix = element.getTransformToElement(svg);                
                // // the new center
                // var newx = (bbox.x + bbox.width/2)//*matrix.a + matrix.e;
                // var newy = (bbox.y + bbox.height/2)//*matrix.d + matrix.f;  
                // // the corresponding top left corner in the current scale
                // var absolute_offset_x = vbox[0] + newx - cx;
                // var absolute_offset_y = vbox[1] + newy - cy;
                // // the new scale
                // var scale = bbox.width*matrix.a/vbox[2] * 1.5; 
                // var scaled_offset_x = absolute_offset_x + vbox[2]*(1-scale)/2;
                // var scaled_offset_y = absolute_offset_y + vbox[3]*(1-scale)/2;
                // var scaled_width = vbox[2]*scale;
                // var scaled_height = vbox[3]*scale;
                // //document.getElementById('diagrams-obj').setAttribute("viewBox", [scaled_offset_x, scaled_offset_y, scaled_width, scaled_height].join(' '))
                // document.getElementById('diagrams-obj').setAttribute("viewBox", ""+scaled_offset_x+" "+scaled_offset_y+" "+scaled_width+" "+scaled_height);                
            } 
        }, 
        drawShape(kind) {
            statusMessage.innerText = 'Click position to add shape'
            this.row = 0
            this.col = 0
            this.drawingMode = true            
            let me = this
            let svgShape = kind
            let colorfill = '#e1f0f5'
            var isDrawing = true
            var svg = document.getElementsByTagName('svg')[0]
            var backface = document.getElementById('backface')   
            
            var css = ((svgShape == 'sector') ? 'path' : svgShape) +':hover{ cursor: move }'
            var style = document.createElement('style')
            if (style.styleSheet) {
                style.styleSheet.cssText = css;
            } else {
                style.appendChild(document.createTextNode(css));
            }

            function insertZonePath(posX, posY, color) 
            {
                me.index ++
                var element = document.createElementNS('http://www.w3.org/2000/svg', 'g');
                element.id = "frame"+me.index
                element.appendChild(style)                                                     
                var x1 = posX
                var y1 = posY
                var x2 = x1 + 100
                var y2 = y1 + 100
                var strM = "M "+x1+" "+y1+" L "+x1 + " "+y2+" L "+x2 + " "+y2+" L "+x2 + " "+y1+" Z" 
                var newpath = document.createElementNS('http://www.w3.org/2000/svg', 'path')
                newpath.id = "zone"+me.index
                newpath.setAttributeNS(null, "d", strM)
                newpath.setAttributeNS(null,"fill", color)
                newpath.classList.add("zone")
                element.appendChild(newpath)                  
                backface.appendChild(element)                                               
            }  
            function insertSectorPath(posX, posY, color) 
            {
                let degrees = 30                
                let h = 100
                let w = 300
                let curve1 = -1*w/4
                var len = h*Math.tan(degrees * Math.PI / 180)
                let curve2 = -1*(w-len*2)/4
                me.index ++
                var element = document.createElementNS('http://www.w3.org/2000/svg', 'g');
                element.id = "frame"+me.index                
                element.appendChild(style)                                                     
                var x1 = posX
                var y1 = posY
                var x2 = x1 + w
                var x3 = x2 - len
                var x4 = x1 + len
                var y2 = y1 + h
                let wx1x2 = x2-x1
                let wx2x3 = x3-x2
                let wx4x3 = x4-x3
                let wx1x4 = x1-x4
                let hd = y2 - y1
                let hz = y1 - y2
                var d = 'M '+x1+' '+y1
                    +' q '+wx1x2/2+' '+curve1+' '+wx1x2+' 0' 
                    +' l '+wx2x3 + ' '+hd
                    +' q '+wx4x3/2+' '+curve2+' '+wx4x3+' 0'
                    +' l '+wx1x4 + ' '+hz                
                var newpath = document.createElementNS('http://www.w3.org/2000/svg', 'path')
                newpath.id = "asector"+me.index
                newpath.setAttributeNS(null, "d", d)
                newpath.setAttributeNS(null,"fill", color)
                newpath.classList.add("zone")
                element.appendChild(newpath)                  
                backface.appendChild(element)   
                me.mapData.push({id: newpath.id, width: w, height: h, angle: degrees*2, x1: x1, y1: y1})                                              
            }              
            function insertRect(posX, posY, color)
            {
                me.index ++
                var element = document.createElementNS('http://www.w3.org/2000/svg', 'g');
                element.id = "frame"+me.index
                element.appendChild(style)                
                let shape = document.createElementNS("http://www.w3.org/2000/svg", 'rect');
                shape.id = "rect"+me.index
                shape.setAttributeNS(null, 'x', posX);
                shape.setAttributeNS(null, 'y', posY);
                shape.setAttributeNS(null, 'width', 132);
                shape.setAttributeNS(null, 'height', 100);
                shape.setAttributeNS(null, 'fill', color);
                element.appendChild(shape)
                backface.appendChild(element);
            } 
            function insertCircle(posX, posY, color)
            {
                me.index ++
                var element = document.createElementNS('http://www.w3.org/2000/svg', 'g');
                element.id = "frame"+me.index
                element.appendChild(style)                
                let shape = document.createElementNS("http://www.w3.org/2000/svg", 'circle');
                shape.id = "circle"+me.index
                shape.setAttributeNS(null, 'cx', posX);
                shape.setAttributeNS(null, 'cy', posY);
                shape.setAttributeNS(null, 'r', 50);
                shape.setAttributeNS(null, 'fill', color)
                element.appendChild(shape)
                backface.appendChild(element)
            }                                
            svg.addEventListener('click', event =>
            {
                let bound = svg.getBoundingClientRect();
                
                let style = getComputedStyle(svg);
                
                let paddingLeft = parseFloat(style['padding-left'].replace('px', ''));
                let paddingTop = parseFloat(style['padding-top'].replace('px', ''));

                let x = event.clientX - bound.left - svg.clientLeft - paddingLeft;
                let y = event.clientY - bound.top - svg.clientTop - paddingTop;
                
                if(event.x === undefined)
                {
                    x -= parseFloat(style['border-left-width'].replace('px', ''));
                    y -= parseFloat(style['border-top-width'].replace('px', ''));
                }
                
                let width = svg.width.baseVal.value;
                let height = svg.height.baseVal.value;
                
                if(x < 0 || y < 0 || x >= width || y >= height)
                {
                    this.drawingMode = false
                    isDrawing = false
                    return;
                }  
                 
                if (isDrawing) { 
                    if (svgShape == 'path') {           
                        insertZonePath(x, y, colorfill)
                    } else if (svgShape == 'rect') {
                        insertRect(x, y, colorfill)
                    } else if (svgShape == 'circle') {
                        insertCircle(x, y, colorfill)
                    } else if (svgShape == 'sector') {
                        insertSectorPath(x, y, colorfill)
                    }
                }
                isDrawing = false
                this.drawingMode = false
                statusMessage.innerText = 'draft'
            }) 
        }, 
        addZoneShape() {          
            this.drawShape('path')                     
        }, 
        addSectorShape() {            
            this.drawShape('sector') 
        },  
        addCircleShape() {          
            this.drawShape('circle') 
        },         
        addRectShape() {            
            this.drawShape('rect') 
        },     
        addSeatRows() {
            this.col = 0
            this.row = 0            
            this.drawingMode = true  
            var isDeterminingSize = false      
            var x1 = 0
            var y1 = 0 
            var xrange = 0
            var yrange = 0  
            var svg = document.querySelector('svg')
            var pt = svg.createSVGPoint()
            document.getElementById("div-SeatRows").style.display = "block"
            // function cursorPoint(evt){
            //     pt.x = evt.clientX; pt.y = evt.clientY;
            //     return pt.matrixTransform(svg.getScreenCTM().inverse());
            // }                    
            //mouse down start determining seat rows size
            document.getElementById('diagrams-obj').addEventListener("mousedown", (e) => {
                //var loc = cursorPoint(e) 
                this.x1 = e.layerX //e.clientX e.offseX
                this.y1 = e.layerY  //e.clientY e.offsetY
                var element = document.createElementNS('http://www.w3.org/2000/svg', 'rect');
                element.id="zone_tmp"
                element.setAttributeNS(null,'x', this.x1)
                element.setAttributeNS(null,'y', this.y1)
                // var colour = '#bbccdd'
                element.setAttributeNS(null,'width', 0)
                element.setAttributeNS(null,'height',0)
                //element.setAttributeNS(null,'fill','#e1f0f5')
                // element.setAttributeNS(null,'style.stroke-width', 2)
                // element.setAttributeNS(null,'style.stroke', colour)
                document.getElementById('diagrams-obj').appendChild(element);
                if (this.drawingMode) {
                    isDeterminingSize = true
                }                
            }) 
            //mousemove catch seat rows size if isDeterminingSize
            document.getElementById('diagrams-obj').addEventListener("mousemove", (e) => {
                //var loc = cursorPoint(e)
                if (this.drawingMode && isDeterminingSize) {               
                    xrange = e.layerX - this.x1
                    if (xrange < 0) {
                        xrange = -1*xrange   
                    }
                    yrange = e.layerY - this.y1
                    if (yrange < 0) {
                        yrange = -1*yrange
                    } 
                    if (isDeterminingSize && this.drawingMode) {
                    ///if (xrange > 0 && yrange > 0 && this.determineScale && this.drawingMode) {
                        // var strM = "M"+this.y1+" "+this.x1 + " L"+this.y1+" "+e.layerX + " L"+e.layerY+" "+e.layerX+" L"+e.layerY+" "+this.x1 +" Z" 
                        // document.getElementById('new-path').setAttributeNS(null,'d', strM) 
                        document.getElementById('zone_tmp').setAttributeNS(null,'width', xrange)
                        document.getElementById('zone_tmp').setAttributeNS(null,'height',yrange)
                        var dimension = this.radius*2
                        if (xrange > (dimension + this.stretchCol)) {
                            this.col = Math.floor(xrange/(dimension + this.stretchCol))   
                        } else {
                            this.col = 0   
                        }
                        if (yrange > (dimension + this.stretchRow)) {
                            this.row = Math.floor(yrange/(dimension + this.stretchRow))   
                        } else {
                            this.row = 0   
                        }                    
                        var coor = " " + this.row + " X " + this.col + " / " +(this.row*this.col)+" seats";
                        document.getElementById("rowCol").innerHTML = coor;   
                    }               
                }
            }) 
            //mouseup end determining size and draw seat rows      
            document.getElementById('diagrams-obj').addEventListener("mouseup", (e) => {   
                isDeterminingSize = false               
                if (this.drawingMode) {
                    var zone_tmp = document.getElementById('zone_tmp');
                    document.getElementById('diagrams-obj').removeChild(zone_tmp);
                    if (this.row > 0 && this.col > 0) {             
                        var css = 'seat{ fill: #ec008c } circle:hover{ opacity: 0.5 } path:hover{ cursor: move }';
                        var style = document.createElement('style');
                        if (style.styleSheet) {
                            style.styleSheet.cssText = css;
                        } else {
                            style.appendChild(document.createTextNode(css));
                        }
                        this.index ++
                        var element = document.createElementNS('http://www.w3.org/2000/svg', 'g');
                        element.id = "frame"+this.index
                        element.setAttributeNS(null,"fill",'#ec008c')
                        element.appendChild(style)
                        var dimension = this.radius * 2;   
                        ///frame off seat rows
                        var x1 = this.x1 - (dimension-2)
                        var y1 = this.y1 - (dimension-2)
                        var x2 = this.x1 + (dimension+this.stretchCol)*this.col - this.radius
                        var y2 = this.y1 + (dimension+this.stretchRow)*this.row - this.radius
                        var strM = "M"+x1+" "+y1+" L"+x1 + " "+y2+" L"+x2 + " "+y2+" L"+x2 + " "+y1+" Z" 
                        var newpath = document.createElementNS('http://www.w3.org/2000/svg', 'path')
                        newpath.id = "sRows"+this.index
                        newpath.setAttributeNS(null, "d", strM)
                        newpath.setAttributeNS(null, "fill-opacity", 0)  
                        newpath.setAttributeNS(null, "fill", "white")                  
                        element.appendChild(newpath)
                        ///draw seats                 
                        var y = this.y1
                        for (var i=0; i < this.row; i++) {
                            var cy = y + i * dimension
                            y = y + this.stretchRow
                            var x = this.x1
                            for (var j=0; j < this.col; j++) {
                                var cx = x + j*dimension 
                                x = x + this.stretchCol                            
                                var seat = document.createElementNS('http://www.w3.org/2000/svg', 'circle'); 
                                seat.id = "g"+this.index+".seat"+(i+1)+"-"+(j+1)
                                seat.setAttributeNS(null,'r', this.radius);           
                                seat.setAttributeNS(null,'cx',cx);
                                seat.setAttributeNS(null,'cy',cy);
                                seat.classList.add("seat");
                                // var colour = '#FCBA03';
                                // seat.setAttributeNS(null,'style.fill',colour);
                                var seatName = document.createElementNS('http://www.w3.org/2000/svg', 'text');
                                seatName.id = "g"+this.index+".lbl"+(i+1)+"-"+(j+1)
                                //caculate x to text align-center seat
                                var tx = cx-1.3
                                if ((j+1) == 10) {
                                    tx = cx-this.radius+2     
                                } else if ((j+1) == 11) {
                                    tx = cx-2.5
                                } else if ((j+1) > 19) {
                                    tx = cx-this.radius+2.5   
                                } else if ((j+1) > 11) {
                                    tx = cx-this.radius+2 
                                } else if ((j+1) > 1) {                          
                                tx = cx-1.5       
                                }
                                //seat number label
                                seatName.setAttributeNS(null,"x", tx)
                                seatName.setAttributeNS(null,"y", cy+this.radius-3) 
                                seatName.setAttributeNS(null,'font-family','sans-serif')
                                seatName.setAttributeNS(null,'font-size',this.radius);
                                seatName.setAttribute('fill', '#ffffff');
                                seatName.textContent = (j+1)
                                // var seatObj = {"row":(i+1),"seat":(j+1)}
                                // this.seatRows.seats.push(seatObj)
                                element.appendChild(seat);
                                element.appendChild(seatName);                                
                            }  
                        }
                        var note = document.createElementNS('http://www.w3.org/2000/svg', 'text')
                        note.id = "g"+this.index+".txt"
                        note.textContent = ''+this.row+'-'+this.col
                        note.setAttributeNS(null,"x", (x1+x2)/2)
                        note.setAttributeNS(null,"y", (y1+y2)/2)   
                        note.setAttribute('fill', 'none');                      
                        element.appendChild(note)
                        document.getElementById('diagrams-obj').appendChild(element);                                       
                    }         
                }             
                this.drawingMode = false
                this.col = 0
                this.row = 0                  
            });          
        },        
        addRectangleTable() {
            let me = this
            this.row = 0
            this.col = 0
            this.drawingMode = true
            var svg = document.getElementsByTagName('svg')[0]
            var isDrawing = true
            statusMessage.innerText = 'Click position to add table'
            function insertRect(posX, posY, color) 
            {
                var css = 'rect:hover{ cursor: move }';
                var style = document.createElement('style');
                if (style.styleSheet) {
                    style.styleSheet.cssText = css;
                } else {
                    style.appendChild(document.createTextNode(css));
                } 
                me.index ++
                var element = document.createElementNS('http://www.w3.org/2000/svg', 'g');
                element.id = "frame"+me.index
                element.appendChild(style)                                
                let s = me.radius*4 
                let rectTable = document.createElementNS("http://www.w3.org/2000/svg", 'rect')
                rectTable.id = 'g'+me.index+'.table'+me.index
                rectTable.setAttributeNS(null, 'x', posX)
                rectTable.setAttributeNS(null, 'y', posY)
                rectTable.setAttributeNS(null, 'rx', 5)
                rectTable.setAttributeNS(null, 'ry', 5)
                rectTable.setAttributeNS(null, 'width', s)
                rectTable.setAttributeNS(null, 'height', s)
                rectTable.setAttributeNS(null, 'fill', '#35c3b8')
                element.appendChild(rectTable)
                let mx = posX + s/2
                let my = posY + s/2
                for (let i=1; i < 5; i++) {
                    var cx = 0
                    var cy = 0
                    if (i==1) {
                        cx = mx - (me.radius*3 + 2)
                        cy = my
                    } else if (i==2) {
                        cx = mx
                        cy = my - (me.radius*3 + 2)
                    } else if (i==3) {
                        cx = mx + (me.radius*3 + 2)
                        cy = my
                    } else if (i==4) {
                        cx = mx
                        cy = my + (me.radius*3 + 2)
                    }          
                    var seat = document.createElementNS('http://www.w3.org/2000/svg', 'circle'); 
                    seat.id = "g.tbl"+me.index+".seat"+i
                    seat.setAttributeNS(null,'r', me.radius);           
                    seat.setAttributeNS(null,'cx',cx);
                    seat.setAttributeNS(null,'cy',cy);
                    seat.setAttributeNS(null,'fill','#ec008c');
                    seat.classList.add("seat");
                    var seatName = document.createElementNS('http://www.w3.org/2000/svg', 'text');
                    seatName.id = "g.tbl"+me.index+".lbl"+i
                    var tx = cx-1.3
                    //seat number label
                    seatName.setAttributeNS(null,"x", tx)
                    seatName.setAttributeNS(null,"y", cy+me.radius-3) 
                    seatName.setAttributeNS(null,'font-family','sans-serif')
                    seatName.setAttributeNS(null,'font-size',me.radius);
                    seatName.setAttribute('fill', '#ffffff');
                    seatName.textContent = i
                    element.appendChild(seat);
                    element.appendChild(seatName);                    
                }
                svg.appendChild(element)
            } 
           svg.addEventListener('click', event =>
            {
                let bound = svg.getBoundingClientRect();
                
                let style = getComputedStyle(svg);
                
                let paddingLeft = parseFloat(style['padding-left'].replace('px', ''));
                let paddingTop = parseFloat(style['padding-top'].replace('px', ''));

                let x = event.clientX - bound.left - svg.clientLeft - paddingLeft;
                let y = event.clientY - bound.top - svg.clientTop - paddingTop;
                
                if(event.x === undefined)
                {
                    x -= parseFloat(style['border-left-width'].replace('px', ''));
                    y -= parseFloat(style['border-top-width'].replace('px', ''));
                }
                
                let width = svg.width.baseVal.value;
                let height = svg.height.baseVal.value;
                
                if(x < 0 || y < 0 || x >= width || y >= height)
                {
                    this.drawingMode = false
                    isDrawing = false
                    return;
                }  
                 
                if (isDrawing) {            
                    insertRect(x, y, 'white')
                }
                isDrawing = false
                this.drawingMode = false
                statusMessage.innerText = 'draft'
            })                     
        },       
        addTilteToShape() {
            // var middleX = x1 + (xrange / 2)
            // var middleY = y1 + (yrange / 2)                    
            // var zoneName = document.createElementNS('http://www.w3.org/2000/svg', 'text');
            // zoneName.id = "z"+this.index+".lbl"+(i+1)+"-"+(j+1)  
            // zoneName.setAttributeNS(null,'font-family','sans-serif')
            // zoneName.setAttributeNS(null,'font-size',dimension);
            // zoneName.setAttribute('fill', 'black');                        
            // zoneName.textContent = 'new zone' 
            // zoneName.setAttributeNS(null,"x", middleX)
            // zoneName.setAttributeNS(null,"y", middleY)  
            //anchor: 'middle'                       
            // element.appendChild(zoneName) 
            // document.getElementById('diagrams-obj').appendChild(element);             
        },
        addRowAlphabetLabel() {   
            var lbCount = 0
            var j = 0         
            var labels = ["A","B","C","D","E","F","G","H"]//,"I","J","K","L","M","N","O","P","Q","R","S"];   
            var curFrameId = "frame"+this.index         
            var y = this.y1;
            var dimension = this.radius * 2;
            var x = this.x1 - (dimension + 8);
            for (var i=0; i < this.row; i++) {
                var ty = y + i * dimension
                var rowLabel = document.createElementNS('http://www.w3.org/2000/svg', 'text');
                rowLabel.setAttributeNS(null,"x", x)
                rowLabel.setAttributeNS(null,"y", ty+this.radius-2) 
                rowLabel.setAttributeNS(null,'font-size', dimension-2);
                rowLabel.setAttribute('fill', '#000')           
 
                if (i < labels.length) {
                    rowLabel.textContent = labels[j]//innerHTML = labels[i]  
                } else {
                   rowLabel.textContent = labels[j]+lbCount         
                }
                y = y + 3  
                document.getElementById(curFrameId).appendChild(rowLabel); 
                j++
                if (j >= labels.length) {
                    j = 0
                    lbCount++
                }                              
            }
        },
        addRowNumberLabel() {

            var x = this.x1;
            var y = this.y1;
            for (var i=0; i < this.row; i++) {
                var ty = y + i * dimension
                var rowLabel = document.createElementNS('http://www.w3.org/2000/svg', 'text');
                rowLabel.setAttributeNS(null,"x", x)
                rowLabel.setAttributeNS(null,"y", ty+this.radius-2) 
                rowLabel.setAttributeNS(null,'font-size', dimension-2);
                rowLabel.setAttribute('fill', '#000');
                rowLabel.textContent = (i+1) 
                y = y + 3 
                //document.getElementById("zoneId").appendChild(rowLabel); 
            }
        },
        makeCurveSeatRows() {
            let me = this
            var seatCol = 0
            var rowCount= 0
            var id = this.selectionId
            id = id.replace('sRows','') 
            var note = document.getElementById('g'+id+'.txt')
            if (note) {
                var para = note.innerHTML
                rowCount = Number(para.slice(0,para.indexOf('-')))        
                seatCol  = Number(para.slice(para.indexOf('-')+1,para.length)) 
            }
            if (seatCol == 0 || rowCount == 0) {
                return
            }
            function getQBezierControlPoint(startPt,endPt,curvature) {
                var x  = (startPt.x + endPt.x) /2 
                var cy = (startPt.y + endPt.y) /2
                var y  = cy - curvature
                return( {x:x,y:y} )
            }
            function getQuadraticBezierXYatPercent(startPt,controlPt,endPt,percent) {
                var x = Math.pow(1-percent,2) * startPt.x + 2 * (1-percent) * percent * controlPt.x + Math.pow(percent,2) * endPt.x; 
                var y = Math.pow(1-percent,2) * startPt.y + 2 * (1-percent) * percent * controlPt.y + Math.pow(percent,2) * endPt.y; 
                return( {x:x,y:y} )
            }
            function resetSeatsOfQuadraticBezier(startPt,endPt,row) {
                var percent = 1 / seatCol 
                var controlPt = getQBezierControlPoint(startPt,endPt,me.curve*2)                                                 
                for (let num = 2; num < seatCol; num++) {
                    var seatId = "g"+id+".seat"+row+"-"+num
                    var seat = document.getElementById(seatId) 
                    var pointc = getQuadraticBezierXYatPercent(startPt,controlPt,endPt,percent*num)
                    seat.setAttributeNS(null,'cx',pointc.x)
                    seat.setAttributeNS(null,'cy',pointc.y) 

                    var labelId = "g"+id+".lbl"+row+"-"+num
                    var seatLabel = document.getElementById(labelId)                    
                    //caculate x to text align-center seat
                    var tx = pointc.x-1.3
                    if (num == 10) {
                        tx = pointc.x-me.radius+2     
                    } else if (num == 11) {
                        tx = pointc.x-2.5
                    } else if (num > 19) {
                        tx = pointc.x-me.radius+2.5   
                    } else if (num > 11) {
                        tx = pointc.x-me.radius+2 
                    } else if (num > 1) {                          
                        tx = pointc.x-1.5       
                    } 
                    seatLabel.setAttributeNS(null,"x", tx)
                    seatLabel.setAttributeNS(null,"y", pointc.y+me.radius-3)                                        
                }              
            }
            //replace cy,cy seats              
            for (let i = 1; i <= rowCount; i++) {                          
                var seatBeg = document.getElementById('g'+id+'.seat'+i+'-1') 
                var seatEnd = document.getElementById('g'+id+'.seat'+i+'-'+seatCol)
                var startPt = {x: Number(seatBeg.getAttributeNS(null,'cx')), y: Number(seatBeg.getAttributeNS(null,'cy'))}
                var endPt   = {x: Number(seatEnd.getAttributeNS(null,'cx')), y: Number(seatEnd.getAttributeNS(null,'cy'))}  
                resetSeatsOfQuadraticBezier(startPt,endPt,i)                   
            }
        },                      
        removeElement() {
            var element = document.getElementById(this.selectionId)
            var parentElement = element.parentElement
            if (parentElement) {
                //delete one seat       
                if (this.selectionId.indexOf('seat')>=0) {
                    var labelDom = document.getElementById(this.selectionId.replace('seat','lbl'))
                    parentElement.removeChild(labelDom)  
                    parentElement.removeChild(element)        
                }
                //delete seat-rows 
                else if (this.selectionId.indexOf('sRows')>=0) {
                    parentElement.innerHTML = ''
                    document.getElementById('diagrams-obj').removeChild(parentElement)    
                } 
                //delete table
                else if (this.selectionId.indexOf('table') >= 0) {
                    parentElement.innerHTML = ''
                    document.getElementById('diagrams-obj').removeChild(parentElement)                      
                } else {
                    document.getElementById('backface').removeChild(parentElement)     
                }
            } else {    
                document.getElementById('diagrams-obj').removeChild(element)
            }
            this.selectionId = null
        }, 
        svgZoomInOutMove() {
            let me = this
            // document.getElementById('diagrams-obj').setAttribute("viewBox", "0 0 "+this.cur_width+" "+this.cur_height)   
            var svgImage = document.getElementById("diagrams-obj");
            var svgContainer = document.getElementById("seatmap");

            var viewBox = {x:0,y:0,w:svgImage.clientWidth,h:svgImage.clientHeight};
            svgImage.setAttribute('viewBox', `${viewBox.x} ${viewBox.y} ${viewBox.w} ${viewBox.h}`);
            const svgSize = {w:svgImage.clientWidth,h:svgImage.clientHeight};
            var isPanning = false;
            var startPoint = {x:0,y:0};
            var endPoint = {x:0,y:0};;
            var scale = 1;

            svgContainer.onmousewheel = function(e) {
                if (!me.drawingMode) {
                    e.preventDefault();
                    var w = viewBox.w;
                    var h = viewBox.h;
                    var mx = e.offsetX;//mouse x  
                    var my = e.offsetY;    
                    var dw = w*Math.sign(e.deltaY)*0.05;
                    var dh = h*Math.sign(e.deltaY)*0.05;
                    var dx = dw*mx/svgSize.w;
                    var dy = dh*my/svgSize.h;
                    viewBox = {x:viewBox.x+dx,y:viewBox.y+dy,w:viewBox.w-dw,h:viewBox.h-dh};
                    scale = svgSize.w/viewBox.w;
                    statusMessage.innerText = 'zoom '+`${Math.round(scale*100)/100}`;
                    svgImage.setAttribute('viewBox', `${viewBox.x} ${viewBox.y} ${viewBox.w} ${viewBox.h}`);
                    this.zoomMode = true
                }
            }

            svgContainer.onmousedown = function(e){
                if (!me.drawingMode) {
                    isPanning = true;
                    startPoint = {x:e.x,y:e.y};
                    if (e.target.id.indexOf('seat') >= 0) {
                        isPanning = false;   
                    }   
                }
            }

            svgContainer.onmousemove = function(e){
                if (me.actTransform) {
                    return
                }
                if (isPanning && me.selectObjKind === 'map' && !me.drawingMode){
                    endPoint = {x:e.x,y:e.y};
                    var dx = (startPoint.x - endPoint.x)/scale;
                    var dy = (startPoint.y - endPoint.y)/scale;
                    var movedViewBox = {x:viewBox.x+dx,y:viewBox.y+dy,w:viewBox.w,h:viewBox.h};
                    svgImage.setAttribute('viewBox', `${movedViewBox.x} ${movedViewBox.y} ${movedViewBox.w} ${movedViewBox.h}`);
                }
            }

            svgContainer.onmouseup = function(e){
                if (isPanning && !me.drawingMode){ 
                    endPoint = {x:e.x,y:e.y};
                    var dx = (startPoint.x - endPoint.x)/scale;
                    var dy = (startPoint.y - endPoint.y)/scale;
                    viewBox = {x:viewBox.x+dx,y:viewBox.y+dy,w:viewBox.w,h:viewBox.h};
                    svgImage.setAttribute('viewBox', `${viewBox.x} ${viewBox.y} ${viewBox.w} ${viewBox.h}`);
                    isPanning = false;
                }
            }

            svgContainer.onmouseleave = function(e){
                isPanning = false;
            }
        },
        mountToZone() {
            var svg = document.querySelector("svg"); 
            function convertCoords(elem,x,y) {

                var offset = svg.getBoundingClientRect();

                var matrix = elem.getScreenCTM();

                return {
                    x: (matrix.a * x) + (matrix.c * y) + matrix.e - offset.left,
                    y: (matrix.b * x) + (matrix.d * y) + matrix.f - offset.top
                };
            }
            //child element          
            var domZoneElemnt = document.querySelector('#'+this.zoneSelection.id)
            var curFrameId = "frame"+this.index  
            var domSeatsElement = document.querySelector('#'+curFrameId);
            if (domSeatsElement && domZoneElemnt) {
                var box = domSeatsElement.getBBox()
                var bbox = domZoneElemnt.getBBox()
                var middleX = bbox.x + (bbox.width / 2)
                var middleY = bbox.y + (bbox.height / 2)
                var absoluteCoords = convertCoords(domZoneElemnt, middleX, middleY);
                //resize seat-rows size conform to zone size  
                if (box.width > bbox.width || box.height > bbox.height) {
                    var xRise = box.width > bbox.width ? (bbox.width / box.width) : 0
                    var yRise = box.height > bbox.height ? (bbox.height / box.height) : 0
                    var scale = yRise > xRise ? yRise : xRise
                    if (scale > 0) {                    
                        var transformSize = ' scale('+scale+')'
                        domSeatsElement.setAttribute('transform', transformSize)
                        box = domSeatsElement.getBBox()
                    }
                }
                var cX = box.x + (box.width / 2)
                var cY = box.y + (box.height / 2)
                var offsetCoords = convertCoords(domSeatsElement, cX, cY);
                var xOffset = absoluteCoords.x - offsetCoords.x//absoluteCoords.x > offsetCoords.x ? absoluteCoords.x - offsetCoords.x : offsetCoords.x - absoluteCoords.x
                var yOffset = absoluteCoords.y - offsetCoords.y //absoluteCoords.y > offsetCoords.y ? absoluteCoords.y - offsetCoords.y : offsetCoords.y - absoluteCoords.y
                var transform = domSeatsElement.getAttributeNS(null, "transform")
                if (transform) {
                    var s_translate = transform.indexOf('translate')
                    var e_translate = transform.indexOf(')')
                    if (s_translate >= 0) {
                        transform = transform.slice(e_translate+1,transform.length)   
                    }
                } else {
                    transform = ''   
                }               
                var transformAttr = 'translate(' + xOffset + ',' + yOffset + ') '+ transform;
                domSeatsElement.setAttribute('transform', transformAttr);     
            }          
        },       
        clearZone() {
        },
        addTextToShape() {
            var id = this.selectionId 
            id = id.replace('zone','')
            id = id.replace('rect','')
            id = id.replace('zone','')
            id = id.replace('circle','')  
            id = id.replace('asector','')          
            var textDom = document.getElementById('text'+id)
            if (textDom) {
                textDom.textContent = this.zoneSelection.name
            } else {
                let fontSize = this.zoneSelection.fontSize
                let orginSize = this.radius*2
                var frameId = 'frame'+id 
                var element = document.querySelector('#'+frameId)
                if (element) {
                    var bbox = element.getBBox()
                    var middleX = bbox.x + (bbox.width / 2)
                    var middleY = bbox.y + (bbox.height / 2)                
                    var text = document.createElementNS('http://www.w3.org/2000/svg', 'text')
                    text.id = "text"+id  
                    text.setAttributeNS(null,'text-anchor', 'middle')
                    text.setAttributeNS(null,'font-family','sans-serif')
                    text.setAttributeNS(null,'font-size',orginSize*fontSize)               
                    text.setAttributeNS(null,"x", middleX)
                    text.setAttributeNS(null,"y", middleY)  
                    text.setAttribute('fill', 'black')                    
                    text.textContent = this.zoneSelection.name
                    element.appendChild(text)
                }                
            }
        }, 
        changeFontSize() {
            var id = this.selectionId 
            id = id.replace('zone','')
            id = id.replace('rect','')
            id = id.replace('zone','')
            id = id.replace('circle','')  
            id = id.replace('asector','')          
            var textDom = document.getElementById('text'+id)
            if (textDom) {
                let fontSize = this.zoneSelection.fontSize
                let orginSize = this.radius*2
                textDom.setAttributeNS(null,'font-size',orginSize*fontSize)
            }                
        },
        stretchSeatRows() {
            var dimension = this.radius * 2;                    
            var y = this.y1
            for (var i=0; i < this.row; i++) {
                var cy = y + i * dimension
                y = y + this.stretchRow
                var x = this.x1
                for (var j=0; j < this.col; j++) {
                    var cx = x + j*dimension 
                    x = x + this.stretchCol
                    var seat = document.createElementNS('http://www.w3.org/2000/svg', 'circle'); 
                    seat.id = "seat"+(i+1)+"-"+(j+1)
                    seat.setAttributeNS(null,'r', this.radius);           
                    seat.setAttributeNS(null,'cx',cx);
                    seat.setAttributeNS(null,'cy',cy);
                    seat.classList.add("seat");
                    // var colour = '#FCBA03';
                    // seat.setAttributeNS(null,'style.fill',colour);
                    // var seatName = document.createElementNS('http://www.w3.org/2000/svg', 'text');
                    // seatName.setAttributeNS(null,"x", cx-this.radius+3)
                    // seatName.setAttributeNS(null,"y", cy+this.radius-2) 
                    // seatName.setAttributeNS(null,'font-size',dimension-3);
                    // seatName.setAttribute('fill', '#ffffff');
                    // seatName.textContent = (j+1)
                    var seatObj = {"row":(i+1),"seat":(j+1)}
                    //this.seatRows.seats.push(seatObj)
                    element.appendChild(seat);
                    // element.appendChild(seatName);
                    document.getElementById('diagrams-obj').appendChild(element);
                }  
            }
        },
        rotateShapeObject() { 
            var element = null  
            if (this.selectObjKind === 'seatRows') {
                var seatRowsId = this.selectionId
                seatRowsId = 'frame'+seatRowsId.replace('sRows','') 
                element = document.querySelector('#'+seatRowsId) 
            } else {
                element = document.querySelector('#'+this.selectionId) 
            }
            if (element.id ==='diagrams-obj') {
                return
            }
            var transform = element.getAttributeNS(null, "transform")
            if (transform) {
                var s_translate = transform.indexOf('translate')
                var e_translate = transform.indexOf(')')
                if (s_translate >= 0) {
                    transform = transform.slice(s_translate, e_translate+1)   
                } else {
                    transform = ''    
                }
            } else {
                transform = ''   
            }                             
            var bbox = element.getBBox()
            var middleX = bbox.x + (bbox.width / 2)
            var middleY = bbox.y + (bbox.height / 2)
            var rotate = "rotate("+this.rotate+" "+middleX+","+middleY +")"
            let transformAttr = transform +' '+rotate
            element.setAttribute("transform", transformAttr);           
        },
        changeShapeSize() {
            var element = document.querySelector('#'+this.selectionId)
            if (element.tagName=='rect') {
                element.setAttributeNS(null, "width", this.d_width)
                element.setAttributeNS(null, "height", this.d_height)
            } else if (element.tagName=='circle') {
                element.setAttributeNS(null, "r", this.d_width)                    
            } else if (element.tagName=='path') {
                this.changeAngleShape()   
            }
        },
        changeAngleShape() {
            let degrees = this.angle/2
            let h = Number(this.d_height)
            let w = Number(this.d_width)
            let curve1 = -1*w/4
            var len = h*Math.tan(degrees * Math.PI / 180)
            let curve2 = -1*(w-len*2)/4
            var x1 = this.x1
            var y1 = this.y1   
            if (x1 == 0 || y1 == 0) {
                return
            }                                                   
            var x2 = x1 + w
            var x3 = x2 - len
            var x4 = x1 + len
            // alert(typeof(w))
            // console.log('w: '+w+'/ x1:'+x1+' x2:'+x2+' x3:'+x3+' x4:'+x4)
            var y2 = y1 + h
            let wx1x2 = x2-x1
            let wx2x3 = x3-x2
            let wx4x3 = x4-x3
            let wx1x4 = x1-x4
            let hd = y2 - y1
            let hz = y1 - y2
            var d = 'M '+x1+' '+y1
                +' q '+wx1x2/2+' '+curve1+' '+wx1x2+' 0' 
                +' l '+wx2x3 + ' '+hd
                +' q '+wx4x3/2+' '+curve2+' '+wx4x3+' 0'
                +' l '+wx1x4 + ' '+hz                
            var sector = document.getElementById(this.selectionId)
            sector.setAttributeNS(null, "d", d)                
        },
        transformShape() {
            let me = this    
            // this.actTransform = this.actTransform == 0 ? 1 : 0 
            // transformBtn.innerText = this.actTransform == 1 ? 'End transform' : 'Start transform'
            function getTransOffset() {
                var frameId = me.selectionId
                frameId = frameId.replace('zone','')
                frameId = 'frame'+frameId
                var parent = document.getElementById(frameId)
                var matrix = parent.getAttributeNS(null, "transform")                                 
                if (matrix) {
                    if (matrix.indexOf("translate") >= 0) {
                        var translate = matrix.slice(matrix.indexOf("(")+1, matrix.indexOf(")"))
                        var x = translate.slice(0,translate.indexOf(","))
                        var y = translate.slice(translate.indexOf(",")+1,translate.length)
                        return {"x":Number(x), "y":Number(y)}
                    } else {
                        return null
                    }
                } else {
                    return null
                }
            }
            statusMessage.innerText = this.actTransform == 1 ? 'Click corner dot and drag to transform' : 'draft'
            if (this.actTransform == 1) {               
                var shapePath = document.getElementById(this.selectionId)
                function getMaTrixPos(path) {
                    var strArr = path.split(' ')
                    var matrixPos = []
                    matrixPos.push({x:parseFloat(strArr[1]),y:parseFloat(strArr[2])})
                    matrixPos.push({x:parseFloat(strArr[4]),y:parseFloat(strArr[5])})
                    matrixPos.push({x:parseFloat(strArr[7]),y:parseFloat(strArr[8])})
                    matrixPos.push({x:parseFloat(strArr[10]),y:parseFloat(strArr[11])})
                    return matrixPos
                }
                var drawn = shapePath.getAttribute('d') 
                var loc = getMaTrixPos(drawn)  
                let transOffset = getTransOffset()          
                for (let i=0; i < loc.length; i++) {
                    var cx = loc[i].x
                    var cy = loc[i].y
                    if (transOffset) {
                        cx = loc[i].x + transOffset.x 
                        cy = loc[i].y + transOffset.y 
                    }                    
                    var dot= document.createElementNS('http://www.w3.org/2000/svg', 'circle'); 
                    dot.id = "dot"+i
                    dot.setAttributeNS(null,'r', 4)           
                    dot.setAttributeNS(null,'cx', cx)
                    dot.setAttributeNS(null,'cy', cy)
                    dot.setAttributeNS(null,'fill','#1f4a72')
                    document.getElementById('diagrams-obj').appendChild(dot)
                }
                this.makeTransform(loc)
            } else {
                for (let i=0; i < 4; i++) {
                    var dot = document.getElementById('dot'+i)
                    if (dot) {
                        document.getElementById('diagrams-obj').removeChild(dot)    
                    }
                }    
            }
        },
        makeTransform(shapeData) {
            if (this.actTransform != 1) {
                return            
            }
            let me = this
            var svg = document.getElementsByTagName('svg')[0]           
            var selectedDot = null
            var dotId = 0
            svg.addEventListener('mousedown', event => {startTransform(event)})
            svg.addEventListener('mousemove', event => {transform(event)})
            svg.addEventListener('mouseup', event => {endTransform(event)})
            svg.addEventListener('mouseleave', event => {endTransform(event)})            
            function getMousePosition(evt) {
                var CTM = svg.getScreenCTM();
                return {
                    x: (evt.clientX - CTM.e) / CTM.a,
                    y: (evt.clientY - CTM.f) / CTM.d
                }
            }             
            function startTransform(evt) { 
                if (evt.target.id.indexOf('dot') < 0) {
                    selectedDot = null    
                } else {
                    selectedDot = evt.target 
                    var strId = evt.target.id
                    dotId = Number(strId.replace('dot',''))
                }                                            
            }  
            function transform(evt) {
                if (selectedDot) {                    
                    evt.preventDefault();
                    var coord = getMousePosition(evt);             
                    var cx = coord.x 
                    var cy = coord.y 
                    selectedDot.setAttribute('cx',cx)
                    selectedDot.setAttribute('cy',cy)
                    var ptmatrix = shapeData
                    ptmatrix[dotId].x = cx
                    ptmatrix[dotId].y = cy
                    var d = 'M '
                    for (let i=0; i < ptmatrix.length; i++) {
                        if (i == 0) {
                            d = d + ptmatrix[i].x + ' ' + ptmatrix[i].y
                        } else {
                            d = d + ' L ' + ptmatrix[i].x + ' ' +ptmatrix[i].y
                        }
                    }
                    d = d+' Z' 
                    document.getElementById(me.selectionId).setAttributeNS(null, "d", d)                    
                }                
            }            
            function endTransform(evt) {
                selectedDot = null               
            }                      
        },
        makeDraggable(evt) {   
            if (this.actTransform == 1) {
                return            
            }                     
            let me = this
            var svg = evt.target;            
            var selectedElement = null;
            var offset = null
            var transOffset = null
            var transform = ''

            svg.addEventListener('mousedown', startDrag);
            svg.addEventListener('mousemove', drag);
            svg.addEventListener('mouseup', endDrag);
            svg.addEventListener('mouseleave', endDrag);
            function getMousePosition(evt) {
                var CTM = svg.getScreenCTM();
                return {
                    x: (evt.clientX - CTM.e) / CTM.a,
                    y: (evt.clientY - CTM.f) / CTM.d
                }
            }            
            function startDrag(evt) { 
                var matrix = null
                selectedElement = evt.target.parentElement 
                offset = getMousePosition(evt);
                if (selectedElement) {
                    matrix = selectedElement.getAttributeNS(null, "transform")
                }
                if (matrix) {
                    if (matrix.indexOf("translate") >= 0) {
                        var translate = matrix.slice(matrix.indexOf("(")+1, matrix.indexOf(")"))
                        var x = translate.slice(0,translate.indexOf(","))
                        var y = translate.slice(translate.indexOf(",")+1,translate.length)
                        transOffset = {"x":Number(x), "y":Number(y)}

                        transform = matrix.slice(matrix.indexOf(")")+1,matrix.length)  
                    } else {
                        transform = matrix   
                    }                    
                }  
                if (me.actTransform == 1) {
                    me.actTransform = 0 
                    me.transformShape()       
                }                             
                // offset.x -= parseFloat(selectedElement.getAttributeNS(null, "x"));
                // offset.y -= parseFloat(selectedElement.getAttributeNS(null, "y"));                                         
            }
            function drag(evt) {
                
                if (selectedElement) {                    
                    evt.preventDefault();
                    var coord = getMousePosition(evt);                
                    // selectedElement.setAttributeNS(null, "x", coord.x - offset.x);
                    // selectedElement.setAttributeNS(null, "y", coord.y - offset.y);
                    var x = coord.x - offset.x
                    var y = coord.y - offset.y                    
                    if (transOffset) {                        
                        x = transOffset.x + (coord.x - offset.x)
                        y = transOffset.y + (coord.y - offset.y)                           
                    }                  
                    var transformAttr = transform.length > 0 ? ' translate(' + x + ',' + y + ') '+transform : 'translate(' + x + ',' + y + ')'
                    selectedElement.setAttribute('transform', transformAttr)                    
                }                
            }
            function endDrag(evt) {

                selectedElement = null;               
            }
        }, 
        activeDeactiveComponent() {
            if (this.selectObjKind === 'seatRows') {
                document.getElementById("div-SeatRows").style.display = "block"
                document.getElementById("div-trans-seatrows").style.display = "block"
                //document.getElementById("div-trans-zone").style.display = "none"
                document.getElementById("div-trans-circle").style.display = "none" 
                document.getElementById("div-angle").style.display = "none"
                document.getElementById("div-rotate").style.display = "none"
                document.getElementById("div-trans-size").style.display = "none" 
            } else if (this.selectObjKind === 'zone') {
                document.getElementById("div-SeatRows").style.display = "none"                
                document.getElementById("div-trans-seatrows").style.display = "none"
                //document.getElementById("div-trans-zone").style.display = "block"
                document.getElementById("div-trans-circle").style.display = "none"
                document.getElementById("div-angle").style.display = "none"
                document.getElementById("div-rotate").style.display = "none"
                document.getElementById("div-trans-size").style.display = "none" 
            } else if (this.selectObjKind === 'rect') {
                document.getElementById("div-SeatRows").style.display = "none"
                document.getElementById("div-trans-seatrows").style.display = "none"
                //document.getElementById("div-trans-zone").style.display = "none" 
                document.getElementById("div-trans-circle").style.display = "none"
                document.getElementById("div-angle").style.display = "none"                
                document.getElementById("div-rotate").style.display = "block"
                document.getElementById("div-trans-size").style.display = "block" 
            } else if (this.selectObjKind === 'sector') {   
                document.getElementById("div-SeatRows").style.display = "none"
                document.getElementById("div-trans-seatrows").style.display = "none"
                //document.getElementById("div-trans-zone").style.display = "none"             
                document.getElementById("div-trans-circle").style.display = "none"  
                document.getElementById("div-angle").style.display = "block"  
                document.getElementById("div-rotate").style.display = "block"
                document.getElementById("div-trans-size").style.display = "block"
            } else if (this.selectObjKind === 'circle') {
                document.getElementById("div-trans-seatrows").style.display = "none"
                //document.getElementById("div-trans-zone").style.display = "none" 
                document.getElementById("div-trans-circle").style.display = "block"
                document.getElementById("div-angle").style.display = "none" 
                document.getElementById("div-rotate").style.display = "none"                 
                document.getElementById("div-trans-size").style.display = "none"                
            } else {
                document.getElementById("div-SeatRows").style.display = "none"
                document.getElementById("div-trans-seatrows").style.display = "none"
                //document.getElementById("div-trans-zone").style.display = "none" 
                document.getElementById("div-trans-circle").style.display = "none"
                document.getElementById("div-angle").style.display = "none" 
                document.getElementById("div-rotate").style.display = "none"                 
                document.getElementById("div-trans-size").style.display = "none"                
            }
        },
        createSimpleSeatMap(){
            this.row= 8
            this.col= 8 
            this.drawSimpleSeatMap() 
        },
        drawSimpleSeatMap() {
            let me = this
            let labels = ["A","B","C","D","E","F","G","H","I","J","K","L","M","N"]
            var svg = document.getElementsByTagName('svg')[0]
            var old_seats = document.getElementById('group_sr')
            if (old_seats) {
                svg.removeChild(old_seats)   
            }            
            var lbCount = 0
            var css = 'path:hover{ opacity: 0.8 }'
            var style = document.createElement('style')
            if (style.styleSheet) {
                style.styleSheet.cssText = css;
            } else {
                style.appendChild(document.createTextNode(css));
            }   
            this.index ++
            this.x1 = 200
            this.y1 = 150           
            let size= 40
            // this.stretchCol=size
            // this.stretchRow=size
            let front_w = this.col*size + this.col*6 + size
            var group = document.createElementNS('http://www.w3.org/2000/svg', 'g');
            group.id = "frame"+this.index
            let shape = document.createElementNS("http://www.w3.org/2000/svg", 'rect');
            shape.id = "rect"+this.index
            shape.setAttributeNS(null, 'x', this.x1)
            shape.setAttributeNS(null, 'y', 20)
            shape.setAttributeNS(null, 'width', front_w)
            shape.setAttributeNS(null, 'height', size)
            shape.setAttributeNS(null, 'fill', '#f5f5f5')
            group.appendChild(shape) 
            var text = document.createElementNS('http://www.w3.org/2000/svg', 'text')
            text.id = "text"+this.index  
            text.setAttributeNS(null,'text-anchor', 'middle')
            text.setAttributeNS(null,'font-family','Arial')
            text.setAttributeNS(null,'font-size',16)               
            text.setAttributeNS(null,"x", this.x1+front_w/2)
            text.setAttributeNS(null,"y", 40)  
            text.setAttribute('fill', 'black')                    
            text.textContent = 'FRONT'
            group.appendChild(text)            
            svg.appendChild(group)

            this.index ++    
            var element = document.createElementNS('http://www.w3.org/2000/svg', 'g');
            element.id = "group_sr"
            element.appendChild(style) 

            function insertSeat(posX,posY,row,col) {
                var x1 = posX
                var y1 = posY
                // var x2 = x1 + size
                // var y2 = y1 + size
                var d = "M "+x1+","+y1+" v-30 a10,10 0 0 1 10,-10 h20 a10,10 0 0 1 10,10 v30 z"            
                let seat = document.createElementNS("http://www.w3.org/2000/svg", 'path')
                seat.id = "g"+me.index+".seat"+row+"-"+col
                seat.setAttributeNS(null, 'd', d);
                seat.setAttributeNS(null, 'fill', '#80c2d1')
                element.appendChild(seat)  

                var seatLabel = document.createElementNS('http://www.w3.org/2000/svg', 'text')
                seatLabel.id = "g"+me.index+".lbl"+row+"-"+col
                seatLabel.setAttributeNS(null,"x", posX+20)
                seatLabel.setAttributeNS(null,"y", posY-15) 
                seatLabel.setAttributeNS(null,'text-anchor', 'middle')
                seatLabel.setAttributeNS(null,'font-family','Arial')
                seatLabel.setAttributeNS(null,'font-size', 16);
                seatLabel.setAttribute('fill', 'white')  
                seatLabel.textContent = labels[row-1]+col 
                element.appendChild(seatLabel)                          
            }               
            var py = this.y1
            for (let i=1; i<= this.row; i++) {  
                var px = this.x1                     
                for (let j=1; j <= this.col; j++) {
                    insertSeat(px,py,i,j)  
                    if (j== (this.col/2).toFixed(0)) {
                        px = px + size*2 + 6
                    } else {
                        px = px + size + 6        
                    }
                }         
                if (i== (this.row/2).toFixed(0)) {
                    py = py + size*2 + 6    
                } else {                
                    py = py + size + 6
                }
            }
            svg.appendChild(element)       
        },
        changeColorShape() {
            document.getElementById(this.selectionId).setAttributeNS(null,'fill',this.colorfill)
        },
        initBlankSVG() {                        
            var seatmap_div = document.getElementById('seatmap')            
            var svg = document.getElementById('diagrams-obj')            
            var positionInfo = seatmap_div.getBoundingClientRect()     
            this.eVB_width = positionInfo.width.toFixed(0)
            this.eVB_height= positionInfo.height.toFixed(0) 
            svg.setAttribute('width',  this.eVB_width) 
            svg.setAttribute('height', this.eVB_height)    
            svg.setAttribute("viewBox", [0, 0, this.eVB_width, this.eVB_height].join(' '))            
        }
    },
    mounted() { 
        this.initBlankSVG()  
        this.mouseOverOutEffect()
        //this.svgZoomInOutMove()
        //this.listenerKeyZoomAndMove()               
    },
    watch: { 
                       
    }     
}
</script>
<style lang="scss" scoped>
    .bnePEP {
        display: flex;
        flex-direction: column;
        min-height: 0px;
        flex: 1 1 auto;
    }
    .gRightEditor {
        position: relative;
        height: calc(100vh - 96px);
        flex: 1 1 auto;
        background: rgb(255, 255, 255);
        margin: 0px;
        overflow: hidden;
    } 
    .gLeftInfor {
        position: relative;
        width: 270px;
        min-width: 270px;
        height: calc(100vh - 96px);
        overflow-y: auto;
        background: rgb(255, 255, 255);
        border-left: 1px solid rgba(0, 0, 0, 0.125);
    }  
    .tilteGrItem {
        height: 36px;
        letter-spacing: 1px;
        text-transform: uppercase;
        padding: 6px 12px 0px;
        font: 400 11px / 30px Roboto, Arial, Helvetica, sans-serif;
    }
    .rightItems {
        font: 400 11px / 1 Roboto, Arial, Helvetica, sans-serif;
        display: flex;
        padding: 6px 0px;
    }
    .gOpoHy {
        color: rgb(0, 0, 0);
        padding: 0px 12px 6px;
        font: 400 11px / 1 Roboto, Arial, Helvetica, sans-serif;
    }  
    .kCdYf {
        color: rgb(0, 0, 0);
        width: 95px;
        max-width: 95px;
        padding-right: 12px;
        display: flex;
        -webkit-box-align: center;
        align-items: center;
        height: 24px;
        white-space: nowrap;
        text-overflow: ellipsis;
        flex: 1 0 95px;
        overflow: hidden;
    } 
    .lewZOv {
        display: flex;
        -webkit-box-align: center;
        align-items: center;
    } 
    .fcJrYL {
        width: 100%;
        height: 24px;
        color: rgb(0, 0, 0);
        border-width: 1px;
        border-style: solid;
        border-color: rgba(0, 0, 0, 0.125);
        border-image: initial;
        border-radius: 4px;
        outline: none;
        padding: 0px 5px;
        background: transparent;
        font: 400 11px / 1 Roboto, Arial, Helvetica, sans-serif;
    }    
    .gwozhV {
        display: flex;
    } 
    .gwozhV button:first-child {
        border-radius: 4px 0px 0px 4px;
    } 
    .gwozhV button:last-child {
        border-radius: 0px 4px 4px 0px;
    } 
    .gwozhV button {
        height: 24px;
        width: 37px;
        color: rgb(0, 0, 0);
        border-width: 1px 0px 1px 1px;
        border-style: solid solid solid;
        border-color: rgba(0, 0, 0, 0.125) rgba(0, 0, 0, 0.125) rgba(0, 0, 0, 0.125);
        border-image: initial;
        border-right: 0px;
        border-radius: 0px;
        outline: none;
        padding: 0px;
        background: transparent;
        font: 400 11px / 1 Roboto, Arial, Helvetica, sans-serif;
    }
    .gwozhV button.selected {
        color: rgb(255, 255, 255);
        background: rgb(17, 153, 238);
        border-width: initial;
        border-style: none;
        border-color: initial;
        border-image: initial;
        padding: 0px;
    }                        
    .svg-container {
        display: inline-block;
        position: relative;
        width: 100%;
        padding-bottom: 100%;
        vertical-align: top;
        overflow: hidden;
        background: linear-gradient(#EEE 1px, transparent 2px), linear-gradient(to right, #EEE 1px, transparent 2px);   
        background-size: 1em 1em;
        background-color: #FFF;             
    }
    .el-dropdown-link {
        .icon {
            font-size: 2rem;
        }
        display: flex;
        align-items: center;
        margin-right: 5px; 
        margin-left: 5px; 
        background: #00AEDB;    
        color: #fff;
        padding: 8px;
        border-radius:5px;              
    }
    .lnIXeA{
        height: 30px;
        white-space: nowrap;
        text-overflow: ellipsis;
        border-width: 1px;
        border-style: solid;
        border-color: rgba(0, 0, 0, 0.125);
        margin: 0px 2px 2px 0;
        border-radius: 4px;
        outline: none;
        padding: 0px 23px;
        background: transparent;
        font: 400 11px / 1 Roboto, Arial, Helvetica, sans-serif;
        overflow: hidden;       
    }
    .lnIXeA:hover {
        background: #ebf5fb    
    }
    .lnIXeA.Btn-fullWidth {
        width: 100%;
    }
    .numbox {
        // margin-left:5px; 
        width: 45px;
        border-width: 1px;
        border-style: solid;
        border-color: rgba(0, 0, 0, 0.125);
        border-radius: 4px;
    }
    .checker {
        fill: #35c3b8;
    }
</style>
