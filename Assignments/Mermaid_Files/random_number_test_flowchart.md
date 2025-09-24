:::mermaid
graph LR
    %% --- Style Definitions ---
    classDef mainLoop fill:#ffe4e1,stroke:#b3a0a0,stroke-width:2px;
    classDef gameInstance fill:#e0ffff,stroke:#a0b3b3,stroke-width:2px;
    classDef guessingLoop fill:#f0e6ff,stroke:#a7a0b3,stroke-width:2px;
    classDef playAgain fill:#e6ffe6,stroke:#a0b3a0,stroke-width:2px;

    %% --- Main Program Loop ---
    subgraph "Main Program Loop"
        A[Start] --> B("Initialize play_again = 'y'");
        B --> C{"Is play_again.lower() == 'y'?"};
    end

    %% --- Define Paths from Main Decision ---
    
    %% Game Path (Flows Right)
    C -- Yes --> D;
    
    %% End Path (Flows Down from the main decision)
    C -- No --> Z("Print 'Thank you for playing!'");
    Z --> AA[End];
    
    %% --- Single Game Instance ---
    subgraph "Single Game Instance"
        D["Output Welcome Messages & Rules"];
        D --> E["Generate Random Number 0-100"];
    end
    
    %% --- Guessing Loop ---
    subgraph "Guessing Loop (10 Tries)"
        E --> FOR_LOOP_START("For guess_count from 1 to 10");
        FOR_LOOP_START --> H["Get Valid User Guess"];
        H --> I{"Is guess == random_number?"};
        
        I -- No --> J{"Is this the last guess (guess_count == 10)?"};
        J -- No --> K{"Is guess > random_number?"};
        K -- Yes --> L["Output 'Too high'"];
        K -- No --> N["Output 'Too low'"];
        L --> FOR_LOOP_END("Next guess");
        N --> FOR_LOOP_END;
        FOR_LOOP_END --> FOR_LOOP_START;
    end

    %% --- Game Outcome ---
    I -- Yes --> P["Output 'Success!'"];
    J -- Yes --> Q["Output 'You ran out of tries!'"];

    %% --- Play Again Prompt ---
    subgraph "Play Again Prompt"
        P --> R("Ask 'Play Again?'");
        Q --> R;
        R --> S{"Is input valid ('y' or 'n')?"};
        S -- No --> R;
        S -- Yes --> T["Update play_again variable"];
        T --> C;
    end
    
    %% --- Class Assignments ---
    class A,B,C,Z,AA mainLoop;
    class D,E gameInstance;
    class FOR_LOOP_START,H,I,J,K,L,N,FOR_LOOP_END guessingLoop;
    class P,Q,R,S,T playAgain;

