.. include:: ../global.rst

.. add_script:: https://cdnjs.cloudflare.com/ajax/libs/processing.js/1.6.6/processing.min.js
   :defer:

.. index:: 
    pair: sort; insertion

Insertion Sort Code
=================================

Once again, taking the algorithm from something appropriate for a human to computer pseudocode requires adding more detail. In the code below, ``i`` is equivalent to the black line - it marks the start of the unsorted portion of the list. ``j`` is the right hand - it stores the location of the card that is being inserted into place. The animation of a human used the left hand to point to the card to the left of the one being inserted - here we use ``j - 1`` to indicate the card to the left of the card being inserted. (Since ``j`` is the index of a location, ``j - 1`` gives us the index to its left.)

.. faux_code::

    \     *Note: assume that list already exists*
    1   ``i`` = 2                                   *i marks start of unsorted cards*
    2   repeat until (``i`` > length of the ``list``):
    3       ``j`` = ``i``                              *j is location of current card*
    4       
    5       *Note: swap current card left until it finds a home*
    6       repeat until (``j`` == 1) or (``list[j]`` > ``list[j - 1]``)
    7           *Note: Swap card to the left"*
    8           ``temp`` = ``list[j]``
    9           ``list[j]`` = ``list[j - 1]``
    10          ``currentMin`` = ``list[j]``
    11          ``j`` = ``j`` - 1                      *current card has moved*
    12       
    13      *Move the marker showing start of unsorted portion*
    14      ``i`` = ``i`` + 1  
      
       
Like before, do not worry about memorizing the algorithm, instead use it and the animation below until you can consistently predict the next step before it runs and understand how i and j are getting used.

.. raw:: html

    <div align="center">
    <canvas id="InsertionSort" style="border-style: solid; image-rendering: -webkit-optimize-contrast !important;" tabindex="0" width="580" height="400"></canvas></div>
    <script type="text/processing" data-processing-target="InsertionSort">
        int[] list = new int[10];

        int curi = -1;
        int curj = -1;

        // Setup the Processing Canvas
        void setup() {
          size(600, 400);
          strokeWeight(2);
          noLoop();

          for (int i = 0; i < list.length; i++) {
            list[i] = i + 1;
          }

          reset();
        }

        // Main draw loop
        void draw() {
          // Fill canvas grey
          background(new color("white"));

          

          drawBars();
        }

        int getMax(int[] list) {
          // Find maximum integer
          int max = list[0];
          for (int i = 1; i < list.length; i++) {
            if (max < list[i]) {
              max = list[i];
            }
          }
          return max;
        }

        void drawBars() {
          double h = height * 0.6;
          double w = width - 60;
          double unitWidth = w * 1.0 / list.length;
          int max = getMax(list);
                  stroke(#000000);

          for (int i = 0; i < list.length; i++) {
            if((i <= curi) || curi == list.length - 1)
                fill(#ffffff);
            else
                fill(#C0C0C0); // Color dim gray
            rect(i * unitWidth + 30, height
              - ((list[i] + 1) / max) * h, unitWidth, ((list[i] + 1) / max) * h - 20);
            fill(0, 102, 153); 
            text(list[i], (i + .5) * unitWidth + 22,
              height - ((list[i] + 1) / max) * h - 7); 
            text((i+1), (i + .5) * unitWidth + 22, height - 5); 
          }

          text("index", 5, height - 5); 
          
          drawSingleBars();
                    fill(#677915);
           text("i = " + (curi + 1) + " (current item starting location)", 10, 15); 
           text("j = " + (curj + 1) + " (current item current location)", 10, 30); 
        }

        void drawSingleBars() {
          double h = height * 0.6;
          double w = width - 60;
          double unitWidth = w * 1.0 / list.length;
          int max = getMax(list);
          
          if(curj == 0 || list[curj] >= list[curj-1]) {
             text("Current item sorted, move to next item", 300, 15); 
              stroke(#000000); // Color green
              fill(#96c170); // Color 
              rect(curj * unitWidth + 30, height
                  - (list[curj] + 1) / max * h, unitWidth, ((list[curj] + 1) / max) * h - 20);
          }
          if(curj != 0 && list[curj] < list[curj-1]) {
             text("Current item needs to swap to the left", 300, 15); 
                stroke(#eeee00); 
              fill(#ffffff); // Color green
              rect((curj-1) * unitWidth + 30, height
                  - (list[(curj-1)] + 1) / max * h, unitWidth, ((list[(curj-1)] + 1) / max) * h - 20);
                  stroke(#000000);

              stroke(#000000); // Color green
              fill(#96c170); // Color green
              rect(curj * unitWidth + 30, height
                  - (list[curj] + 1) / max * h, unitWidth, ((list[curj] + 1) / max) * h - 20);

          }
        }


        void reset() {
          shuffleList();
          curi = -1;
          curj = -1;
          swapping = false;
                movingToNext = false;

          // Fill canvas grey
          background(new color("white"));

         }

        void resetMostlySorted() {

          list = {1, 3, 4, 2, 5, 7, 8, 6, 9, 10};
          curi = -1;
          curj = -1;
          swapping = false;
                movingToNext = false;

          // Fill canvas grey
          background(new color("white"));
            
        }

        void shuffleList() {
          for (int i = 0; i < list.length; i++) {
            int index = int(random(list.length));
            int temp = list[index];
            list[index] = list[i];
            list[i] = temp;
          }
        }

        boolean step() {
          if (curi == -1) {curi = 1; curj = 1; draw(); return false;}    //start it
          if (curi == list.length) return true; // Now the list is sorted

         
          if(curj == 0 || list[curj] >= list[curj-1]) {
                //done with this item
                curi++;
                curj = curi;
          } else {
                //slide left
                int temp = list[curj];
                list[curj] = list[curj-1];
                list[curj-1] = temp;
                curj--;
          }


          
          draw();
          return false; // Not sorted yet
        }
    </script>
    <script>
      function step(id) {
        var pjs = Processing.getInstanceById(id);    
        var isSorted = pjs.step();
        if (isSorted) {
          alert("The list is now sorted.\nClick Reset to start over" );        
        }; 
      }
      
      function reset(id) {
        var pjs = Processing.getInstanceById(id);
        pjs.reset();
        pjs.draw();
      }  
      function resetMostlySorted(id) {
          var pjs = Processing.getInstanceById(id);
          pjs.resetMostlySorted();
          pjs.draw();
      }
    </script>
    <p></p> 
    <div align="center">
    <button type="button" onclick="step('InsertionSort')">Step</button>
    <button type="button" onclick="reset('InsertionSort')">Reset</button>
    <button type="button" onclick="resetMostlySorted('InsertionSort')">Reset - Partial Sorted</button>
    <div style="font-size: 80%">Animation based on work by <a href="http://cs.armstrong.edu/liang/animation/animation.html">Y. Daniel Liang</a></div>
    </div>
    <br/>

    
Why would we care about knowing more than one sorting algorithm? Each approach has its own advantages and disadvantages. Try clicking the "Reset - Partially Sorted" button and then run the sort on a list that is almost in order. Compare how insertion sort works on that list with how :ref:`the selection sort <selectionSortAnimation>` does on the same list. Which one is more effective?