Inventory Environment

import numpy as np

class InventoryEnv:
    def __init__(self, max_inventory=100):
        self.max_inventory = max_inventory
        self.inventory = max_inventory // 2

    def step(self, action):
        demand = np.random.poisson(20)
        sales = min(self.inventory, demand)
        self.inventory += action - sales
        self.inventory = max(0, min(self.inventory, self.max_inventory))

        holding_cost = 0.5 * self.inventory
        stockout_cost = 10 * max(0, demand - sales)
        ordering_cost = 2 * action

        reward = sales * 20 - (holding_cost + stockout_cost + ordering_cost)
        state = np.array([self.inventory, demand])
        return state, reward
TRPO Agent (High-Level)

class TRPOAgent:
    def __init__(self):
        pass  # Policy & value network initialization

    def select_action(self, state):
        return np.random.randint(0, 20)  # placeholder

    def update_policy(self, trajectories):
        pass  # TRPO update logic
Training Loop

env = InventoryEnv()
agent = TRPOAgent()

for episode in range(1000):
    state = np.array([env.inventory, 0])
    episode_reward = 0

    for step in range(30):
        action = agent.select_action(state)
        next_state, reward = env.step(action)
        episode_reward += reward
        state = next_state

    if episode % 100 == 0:
        print(f"Episode {episode}, Reward: {episode_reward}")
