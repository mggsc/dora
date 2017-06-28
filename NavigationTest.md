The following is an algorithm implementing the Rapidly Exploring Random Tree (RRT) searches for a path from the initial configuration to the goal configuration by expand a search tree.
The function RRTPlan is the main function and repeatedly calls the three functions -ChooseTarget, Nearest and Extend - until reaching the target configuration.

    Function RRTPlan(configuration: qinit, configuration: qgoal): tree {
        qnearest := qinit
        Initialize a tree T
        while ( the distance between the qnearest and qgoal < threshold ) {
          qtarget := ChooseTarget (qgoal)
          qnearest := Nearest (T, qtarget)
          qextended := Extend (qnearest, qtarget)
          if ( qextended != EmptyConfig ) { Add qextended into T }
        }
        return T
    }

The function ChooseTarget determines whether the target configuration in the next step of expanding is some random configuration or the goal itself. p is a user defined value and for every step,
there is a probability of p that the target configuration is the goal configuration qgoal and a probability of 1-p that the target configuration is some random configuration.

    Function ChooseTarget (configuration: qgoal): configuration {
        Pick a random number uniformly distributed in [0, 1]
        if ( 0 < p < GoalProb ) { return qgoal }
        else if ( GoalProb < p < 1 ) { Return a random configuration }
    }
   
The function Nearest determines the node on the tree that is closest to the target configuration qtarget.

    Function Nearest (tree: T, configuration: qtarget): configuration {
        qnearest := EmptyConfig
        foreach configuration qi in T {
          if ( the distance between qi and qtarget < the distance between qnearest and qtarget )
          { qnearest := qi }
        }
        return qnearest
    }
The function Extend expands the tree towards a target configuration qtarget with a unit distance l. After expanding, it performs collision checks to determine whether the
newly obtained configuration qextended is valid.

    Function Extend (configuration: qnearest, configuration: qtarget): configuration {
        Use some heuristics to extend qnearest towards qtarget by some distance l to generate qextended
        if ( the robot collides with something with qextended ) { return EmptyConfig }
        else { return qextended }
    }

