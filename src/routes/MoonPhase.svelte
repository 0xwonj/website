<script lang="ts">
    let currentdate: Date = new Date();
    let moonRotationDeg: number = getMoonPhaseRotation(currentdate);
    let hemisphere0IsLight: boolean = moonRotationDeg < 180;
    let hemisphere1IsLight: boolean = !hemisphere0IsLight;
  
    function getMoonPhaseRotation(date: Date): number {
      const cycleLength = 29.5305882;
  
      const knownNewMoon = new Date('2022-03-02 18:34:00');
      const secondsSinceKnownNewMoon = (date.getTime() - knownNewMoon.getTime()) / 1000;
      const daysSinceKnownNewMoon = secondsSinceKnownNewMoon / 86400;
      const currentMoonPhasePercentage =
        ((daysSinceKnownNewMoon % cycleLength) + cycleLength) % cycleLength / cycleLength;
  
      return 360 - Math.floor(currentMoonPhasePercentage * 360);
    }
  
    $: {
      moonRotationDeg = getMoonPhaseRotation(currentdate);
      hemisphere0IsLight = moonRotationDeg < 180;
      hemisphere1IsLight = !hemisphere0IsLight;
    }
  </script>
  
  <div class="sphere">
    <div class="hemisphere" class:light={hemisphere0IsLight} class:dark={!hemisphere0IsLight}></div>
    <div class="hemisphere" class:light={hemisphere1IsLight} class:dark={!hemisphere1IsLight}></div>
    <div class="divider" style="transform: rotate3d(0, 1, 0, {moonRotationDeg}deg);"></div>
  </div>
  
  <style>
    .sphere {
      border-radius: 100%;
      width: 250px;
      height: 250px;
      display: flex;
      overflow: hidden;
      align-items: center;
      position: relative;
    }
    
    .sphere:hover {
        box-shadow: 0 0 30px 10px rgba(255, 255, 153, 0.4);
    }
  
    .hemisphere {
      width: 50%;
      height: 100%;
    }
  
    .light {
      background-color: #FFEB8E;
      background-image: radial-gradient(circle, #FFF1B5 0%, #FFEB8E 70%);
    }
  
    .dark {
      background-color: var(--color-bg-0);
    }
  
    .divider,
    .divider::after {
      top: 0;
      left: 0;
      width: 250px;
      height: 250px;
      position: absolute;
      border-radius: 100%;
      transform-style: preserve-3d;
      backface-visibility: hidden;
    }
  
    .divider {
      background-color: var(--color-bg-0);
    }
  
    .divider::after {
      content: '';
      transform: rotateY(180deg);
    }
  </style>
  