# Market Simulation

It is an example of the a file structure and agent implementations using [Market Gym](../market_gym/README.md), as well as the script to run them.

## How to create new agents using Market Gym

The actual implementations should be put in `_agents` folder and imported to `__init__.py` from the same folder. So, we suggest that it should have at least the following files:
```sh
    _agent/
        __init__.py
        agent_frwk.py
        your_agent.py
        risk_model.py
    __init__.py
    agent.py
```

In the current `_agents` folder, there is a file called `agent_frwk.py` that implements a working version of `Agent` class called `BasicAgent` that takes actions randomly. Note that this agent is specialized in interest future contracts (as this whole library), but can be adapted to other instruments. `risk_model.py` was created due to the instruments traded, but it turns out to be a good way to control agent exposure, so you can use a similar structure or not. It is not mandatory. In any way. after implement your agent, `_agent/__init__.py` should have:

```python
    from .agent_frwk import BasicAgent
    from .your_agent import YourAgent
```

Then, include your agent to `agent.py` script. You should modify `Run.set_experiments_options()` and create a new method called `Run._youragentname()`. So, the script should look something like:

```python
    (...)
    # === IMPORT AGENTS TO USE IN THE SIMULATION HERE ===
    from _agents import BasicAgent
    from _agents import YourAgent
    # ==================================================
    (...)
    class Run():
    (...)
    def set_experiments_options(self):
        '''
        '''
        d_experiments = {'_youragent': self._youragent}
        return d_experiments
    def _youragent(self, e, f_aux):
        '''
        '''
        e.set_reward_function('pnl')
        s_log_name = 'youragent'
        if self.s_log_name:
            s_log_name = self.s_log_name
        e.set_log_file(s_log_name)
        s_hedging_on = self._get_from_argv('s_hedging_on')
        b_keep_pos = self._get_from_argv('b_keep_pos')
        b_hedging = self._get_from_argv('b_hedging')
        # check if there are initial positions
        d_initial_pos = self._get_from_argv('d_initial_pos')
        if not d_initial_pos:
            d_initial_pos = {}
        # recover secondary instruments
        s_sec_instr = self._get_from_argv('s_sec_instr')
        s_sec_hedging_on = self._get_from_argv('s_sec_hedging_on')
        # set up the agent
        a = e.create_agent(YourAgent,
                           f_min_time=f_aux,
                           b_include_all_variable_here=b_your_agents,
                           s_hedging_on=s_hedging_on,
                           s_sec_instr=s_sec_instr,
                           s_sec_hedging_on=s_sec_hedging_on,
                           b_hedging=b_hedging,
                           b_keep_pos=b_keep_pos,
                           d_initial_pos=d_initial_pos)
        return a
```

Note that `Run._get_from_argv()` is a method to recover arguments passed through the shell command. After that, you should see your agent when you run at the terminal:

```shell
    $ python -m market_sim.agent -h
```

## What you should know about `_agent/risk_model.py`

bla

## What you should know about `_agent/agent_frwk.py`

bla

## What you should know about `agent.py`

bla