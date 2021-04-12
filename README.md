# agv_maps



# map.html

```
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8" />


  <script type="text/javascript" src="scripts/easeljs.min.js"></script>
  <script type="text/javascript" src="scripts/eventemitter2.min.js"></script>
  <script type="text/javascript" src="scripts/roslib.min.js"></script>
  <script type="text/javascript" src="scripts/ros2d.min.js"></script>
  <script type="text/javascript" src="scripts/nav2d.js"></script>
  <script type="text/javascript" src="scripts/ros3d.min.js"></script>

  <script type="text/javascript" type="text/javascript">
    //document.addEventListener("DOMContentLoaded", function(event) { 
    var ros = new ROSLIB.Ros({
      url: 'ws://localhost:9090'
    });

    ros.on('connection', function () {
      document.getElementById("status").innerHTML = "Connected";





      var viewer = new ROS3D.Viewer({
        divID: 'markers',
        width: 800,
        height: 600,
        antialias: true
      });

      var tfClient = new ROSLIB.TFClient({
        ros: ros,
        angularThres: 0.01,
        transThres: 0.01,
        rate: 10.0,
        fixedFrame: '/map'
      });

      // Setup the marker client.
      /* var markerClient = new ROS3D.MarkerClient({
         ros: ros,
         tfClient: tfClient,
         topic: 'eru/visualization_marker_throttle',
         camera: viewer.camera,
         rootObject: viewer.selectableObjects

       });*/

      var robotLocations = new ROS3D.MarkerArrayClient({
        ros: ros,
        tfClient: tfClient,
        topic: '/visualization_marker_array',
        rootObject: viewer.selectableObjects,

      })

      var robotName_text = new ROS3D.MarkerArrayClient({
        ros: ros,
        tfClient: tfClient,
        topic: '/visualization_marker_array_text',
        rootObject: viewer.selectableObjects,

      })



      var waypoints = new ROS3D.MarkerArrayClient({
        ros: ros,
        tfClient: tfClient,
        topic: '/waypoints_marker_array',
        rootObject: viewer.selectableObjects,

      })

      var waypoints_text = new ROS3D.MarkerArrayClient({
        ros: ros,
        tfClient: tfClient,
        topic: '/waypoints_marker_array_text',
        rootObject: viewer.selectableObjects,

      })







      var gridClient3d = new ROS3D.OccupancyGridClient({
        ros: ros,
        continuous: true,
        rootObject: viewer.scene
      });





    });

    ros.on('error', function (error) {
      document.getElementById("status").innerHTML = "Error";
    });

    ros.on('close', function () {
      document.getElementById("status").innerHTML = "Closed";
    });


    //});
  </script>
</head>

<body>
  <h1>Simple ROS User Interface</h1>
  <p>Connection status: <span id="status"></span></p>
  <p>Last /txt_msg received: <span id="msg"></span></p>



  <div id="markers" class="col-8"></div>


  </div>



</body>

</html>
```
