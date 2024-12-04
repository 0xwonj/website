<script lang="ts">
    let currentDate: Date = new Date();
    let moonRotationDeg: number = getMoonPhaseRotation(currentDate);
    let hemisphere0IsLight: boolean = moonRotationDeg < 180;
    let hemisphere1IsLight: boolean = !hemisphere0IsLight;
    let isLight: boolean = moonRotationDeg > 90 && moonRotationDeg < 270;
  
    function getMoonPhaseRotation(date: Date): number {
      const cycleLength = 29.5305882;
  
      const knownNewMoon = new Date('2022-03-02 18:34:00');
      const secondsSinceKnownNewMoon = (date.getTime() - knownNewMoon.getTime()) / 1000;
      const daysSinceKnownNewMoon = secondsSinceKnownNewMoon / 86400;
      const currentMoonPhasePercentage =
        ((daysSinceKnownNewMoon % cycleLength) + cycleLength) % cycleLength / cycleLength;
      
      return 360 - Math.floor(currentMoonPhasePercentage * 360);
    }
  </script>
  
  <div class="sphere">
    <div class="divider" class:light={isLight} class:dark={!isLight} style="transform: rotate3d(0, 1, 0, {moonRotationDeg}deg);"></div>
    <div class="hemisphere" class:light={hemisphere0IsLight} class:dark={!hemisphere0IsLight}></div>
    <div class="hemisphere" class:light={hemisphere1IsLight} class:dark={!hemisphere1IsLight}></div>
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
      background-color: #fdefa3;
    }
  
    .dark {
      backface-visibility: hidden;
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
    }

    .divider.dark {
      background-color: var(--color-bg-0);
    }
  </style>
  