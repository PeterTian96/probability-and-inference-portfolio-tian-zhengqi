Martingale Strategy Computer Simulation
================
Zhengqi(Peter) Tian
9/1/2021

Reference:
<https://github.com/thomasgstewart/data-science-5620-Fall-2021/blob/master/deliverables/01-roulette.md>

***Background and Operating Characters***
<p>
 As the purpose of the article, We try to use data science tech to
analyze a class roulette strategy, Martingale strategy. As we known that
a roulette table composed of 38 (or 37) evenly sized pockets on a wheel,
including red, black, or green pockets. The payout for a bet on black is
1 dollar for each wagered. Generally, for each sub sequence, no matter
how many losses happened before the final red win, the win wager will
not only offset the previous lost, but also will earn 1 dollar. To know
whether it will work, as a data scientist,applying computer simulation
will help us understand the scenario.
</p>
<p>
 To explain how the computer does simulation, first of all, we need
think over key features in the whole procedure by understating the
operating characteristics in the strategy. Generally, there are two
processes, the single spin of the wheel and martingale wager rule.
combing two processing, we could create a function to replicate series
of plays. To test key properties of the interest, profit or loss, we
need applying stop rule for series of plays. Base on the background, we
have three stopping rules:
</p>

       # three stopping rules
        - The player has W dollars, which are 200 dollar starting budgets and 100 dollar winnings
        - The player goes bankrupt
        - The player completes L wagers (or plays)
      

<p>
 With the clear operating characteristics, there are four Parameters:
</p>

       # four Parameters
       - Starting budget
       - Maximum wager
       - Maximum number of players
       - Winning threshold for stopping

***Simulation Explanations***  
 ***Simulation Function for Spin of the Wheel***
<p>
 Now, we could start the computer simulation step by step. First, the
computer simulation helps us get a random solution of the single spin of
the wheel, as we apply the following code:
</p>

      # Spin of the wheel
      red <- rbinom(1,1,18/38) 
      
      or 
      
      single_spin <- function(){
      possible_outcomes <- c(rep("red",18), rep("black",18), rep("green",2))
      sample(possible_outcomes, 1)
    }

<p>
 The code allow us to receive a random spin result as the red
probability is 18 out of 38.
</p>
 ***Simulation Function for martingale wager rule***
<p>
 The Next task is to consider the stimulation of martingale wager rule.
To complete the whole procedures, we need find the previous outcome. If
the previous outcome negative, we need to know previous wager, max
wager, and current budget as well. Then, we are able to calculate
martingale wager. For martingale wager, if the outcome is
<font  color=Red>“Red”</font>, the wager will be one, otherwise it will
be the minimum amount between twice of the previous wage, max wage, and
the current budget. Here, we have two codes solutions:
</p>

      # The Single Martingale Wager
      {
      proposed_wager <- ifelse(state$previous_win, 1, 2*state$previous_wager)
      wager <- min(proposed_wager, state$M, state$B)
      }
      
      or
      
      {
      if(previous_outcome == "red") return(1)
      min(2*previous_wager, max_wager, current_budget)
      }

<p>
 Now, with the logical, we are going to put all four inputs into the
martingale wager rule and create the function:
</p>

       # The Single Martingale Wager Function
        state$plays <- state$plays + 1
        state$previous_wager <- wager
        if(red){
            # WIN
          state$B <- state$B + wager
          state$previous_win <- TRUE
        }else{
            # LOSE
          state$B <- state$B - wager
          state$previous_win <- FALSE
        }
        state
        }
        
        or 
      
        martingale_wager <- function(previous_wager, previous_outcome, max_wager, current_budget){
        if(previous_outcome == "red") return(1)
        min(2*previous_wager, max_wager, current_budget)
        }
        

 ***Single Game/ Series of ***

 ***Series of Plays***

\#\# See the stopping rule below. Explain to your audience how you used
computer simulation to estimate the average number of plays before
stopping. The code below will need to be modified to calculate this
quantity.

 ***Stop Rules***

 ***Earning and Loss***

## You should explain how you used computer simulation to calculate the average earnings of a gambler that uses this strategy. As part of the explanation, provide a figure (or a series of figures) that show how the gamblers earnings (or losses) evolve over a series of wagers at the roulette wheel. (The x-axis will be the wager number (or play number), the y-axis will be earnings.) The code below provides all the functions you’ll need to calculate average earnings.

 ***Average earning impact based on a chaning parameter***

## Show your audience how changing a parameter of the simulation (see table below) does or does not have an impact on average earnings. A figure would be helpful.

## Be sure to explain the limitations of the simulation; identify simplifications or other sources of uncertainty.

 ***limitations of the simulation***
