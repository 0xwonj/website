<script lang="ts">
    import { onMount } from 'svelte';

    // Constants
    const WIDTH = 15;
    const GRID_SIZE = WIDTH * WIDTH;
    const INITIAL_SHOOTER_INDEX = 202;
    const INITIAL_ALIEN_POSITIONS = [
        0, 1, 2, 3, 4, 5, 6, 7, 8, 9,
        15, 16, 17, 18, 19, 20, 21, 22, 23, 24,
        30, 31, 32, 33, 34, 35, 36, 37, 38, 39
    ];
    const MAX_LASERS = 40;

    // Game state
    let currentShooterIndex: number;
    let squares: Array<{
        isInvader: boolean;
        isShooter: boolean;
        isLaser: boolean;
        isBoom: boolean;
    }>;
    let alienInvaders: number[];
    let aliensRemoved: number[];
    let invadersId: NodeJS.Timeout | null;
    let isGoingRight: boolean;
    let direction: number;
    let results: number;
    let gameStatus: 'START' | 'PLAYING' | 'WIN' | 'GAME_OVER' = 'START';
    let laserCount: number = MAX_LASERS;
    let canShoot: boolean;

    // Game initialization function
    function initializeGame() {
        currentShooterIndex = INITIAL_SHOOTER_INDEX;
        alienInvaders = [...INITIAL_ALIEN_POSITIONS];
        aliensRemoved = [];
        isGoingRight = true;
        direction = 1;
        results = 0;
        laserCount = MAX_LASERS;
        gameStatus = 'PLAYING';
        canShoot = true;

        squares = Array(GRID_SIZE).fill(null).map(() => ({
            isInvader: false,
            isShooter: false,
            isLaser: false,
            isBoom: false
        }));

        draw();
        squares[currentShooterIndex].isShooter = true;

        if (invadersId) clearInterval(invadersId);
        invadersId = setInterval(moveInvaders, 600);
    }

    function draw() {
        for (let i = 0; i < alienInvaders.length; i++) {
            if (!aliensRemoved.includes(i)) {
                squares[alienInvaders[i]].isInvader = true;
            }
        }
        squares = [...squares];
    }

    function remove() {
        for (let i = 0; i < alienInvaders.length; i++) {
            squares[alienInvaders[i]].isInvader = false;
        }
        squares = [...squares];
    }

    function moveInvaders() {
        const leftEdge = alienInvaders[0] % WIDTH === 0;
        const rightEdge = alienInvaders[alienInvaders.length - 1] % WIDTH === WIDTH - 1;
        
        remove();

        if (rightEdge && isGoingRight) {
            for (let i = 0; i < alienInvaders.length; i++) {
                alienInvaders[i] += WIDTH + 1;
                direction = -1;
                isGoingRight = false;
            }
        }

        if (leftEdge && !isGoingRight) {
            for (let i = 0; i < alienInvaders.length; i++) {
                alienInvaders[i] += WIDTH - 1;
                direction = 1;
                isGoingRight = true;
            }
        }

        for (let i = 0; i < alienInvaders.length; i++) {
            alienInvaders[i] += direction;
        }

        draw();

        if (squares[currentShooterIndex].isInvader) {
            gameStatus = 'GAME_OVER';
            if (invadersId) clearInterval(invadersId);
        }

        if (aliensRemoved.length === alienInvaders.length) {
            gameStatus = 'WIN';
            if (invadersId) clearInterval(invadersId);
            results += laserCount;
        }
    }

    function handleKeydown(e: KeyboardEvent) {
        if (gameStatus !== 'PLAYING') {
            if (e.key === 'Enter') initializeGame();
            return;
        }

        if (e.key === 'ArrowLeft' || e.key === 'ArrowRight') {
            squares[currentShooterIndex].isShooter = false;
            
            if (e.key === 'ArrowLeft') {
                if (currentShooterIndex % WIDTH !== 0) currentShooterIndex -= 1;
            }
            if (e.key === 'ArrowRight') {
                if (currentShooterIndex % WIDTH < WIDTH - 1) currentShooterIndex += 1;
            }
            
            squares[currentShooterIndex].isShooter = true;
            squares = [...squares];
        }

        if (e.key === 'ArrowUp') {
            shoot();
        }
    }
    
    function shoot() {
        if (!canShoot || laserCount < 1) return;
        
        canShoot = false;

        setTimeout(() => {
            canShoot = true;
        }, 300);
        laserCount--;

        let laserId: NodeJS.Timeout;
        let currentLaserIndex = currentShooterIndex;

        function moveLaser() {
            squares[currentLaserIndex].isLaser = false;
            currentLaserIndex -= WIDTH;
            
            if (currentLaserIndex >= 0) {
                squares[currentLaserIndex].isLaser = true;

                if (squares[currentLaserIndex].isInvader) {
                    squares[currentLaserIndex].isLaser = false;
                    squares[currentLaserIndex].isInvader = false;
                    squares[currentLaserIndex].isBoom = true;

                    setTimeout(() => {
                        squares[currentLaserIndex].isBoom = false;
                        squares = [...squares];
                    }, 300);
                    
                    clearInterval(laserId);

                    const alienRemoved = alienInvaders.indexOf(currentLaserIndex);
                    aliensRemoved = [...aliensRemoved, alienRemoved];
                    results++;
                }
                
                squares = [...squares];
            } else {
                clearInterval(laserId);
            }
        }

        laserId = setInterval(moveLaser, 100);
    }

    function restart() {
        gameStatus = 'START';
        if (invadersId) clearInterval(invadersId);
    }

    onMount(() => {
        window.addEventListener('keydown', handleKeydown);
        return () => {
            window.removeEventListener('keydown', handleKeydown);
        };
    });
</script>

<div class="game-container">
    {#if gameStatus === 'START'}
        <div class="grid">
            <h1>SPACE INVADERS</h1>
            <p>Press ENTER to Start</p>
        </div>
    {:else if gameStatus === 'PLAYING'}
        <div class="score">Score: {results}</div>
        <div class="laser-count">Laser: {laserCount}</div>
        <button class="restart-button" on:click={restart} aria-label="Restart Game">↻</button>

        <div class="grid">
            {#each squares as square}
                <div 
                    class:invader={square.isInvader} 
                    class:shooter={square.isShooter}
                    class:laser={square.isLaser}
                    class:boom={square.isBoom}>
                </div>
            {/each}
        </div>
    {:else}
        <div class="grid">
            <h1>{gameStatus === 'WIN' ? 'YOU WIN!' : 'GAME OVER'}</h1>
            <p>Score: {results}</p>
            <p>Press ENTER to Restart</p>
        </div>
    {/if}
</div>

<style>
    .game-container {
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 2rem 0 0 0;
        border: solid white 2px;
        background-color: black;
        color: white;
        position: relative;
    }

    .game-container h1 {
        font-size: 2rem;
        margin: 0;
        color: white;
    }

    .score {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        text-align: left;
        font-size: 0.8rem;
    }

    .laser-count {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        text-align: right;
        font-size: 0.8rem;
    }

    .restart-button {
        position: absolute;
        bottom: 0;
        right: 4px;
        font-size: 1.5rem;
        cursor: pointer;
        background: none;
        border: none;
        color: white;
    }

    .grid {
        width: 300px;
        height: 300px;
        display: flex;
        flex-wrap: wrap;
        background-color: black;
        justify-content: center;
        align-items: center;
    }

    .grid div {
        width: 20px;
        height: 20px;
        box-sizing: border-box;
    }

    .grid p {
        font-size: 0.8rem;
        text-align: center;
        margin: 0;
        font-family: var(--font-body);
    }

    :global(.invader) {
        background-color: purple;
        border-radius: 10px;
    }

    :global(.shooter) {
        background-color: green;
        border-radius: 3px;
    }

    :global(.laser) {
        background-color: orange;
    }

    :global(.boom) {
        background-color: red;
    }
</style>