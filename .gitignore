import sys
import math
import itertools

lookup = ' ABCDEFGHIJKLMNOPQRSTUVWXYZ'

runes = list(itertools.repeat(0, 30))
cur_rune = 0
actions = ''


def rune_dist(x, y):
    return min(abs(x - y), abs(x - y + 30), abs(x - y - 30))


def sym_dist(x, y):
    return min(abs(x - y), abs(x - y + 27), abs(x - y - 27))


def move_rune(next_rune):
    is_left = (next_rune < cur_rune and (next_rune + 30 - cur_rune) > (cur_rune - next_rune)) or (
        next_rune > cur_rune and (cur_rune + 30 - next_rune) < (next_rune - cur_rune))
    return ''.join(itertools.repeat('<' if is_left else '>', rune_dist(cur_rune, next_rune)))


def move_sym(next_rune, c):
    cur_sym = runes[next_rune]
    is_minus = (c < cur_sym and (c + 27 - cur_sym) > (cur_sym - c)) or (
        c > cur_sym and (cur_sym + 27 - c) < (c - cur_sym))
    return ''.join(itertools.repeat('-' if is_minus else '+', sym_dist(cur_sym, c)))


def next_action(c):
    c = lookup.index(c)
    move_costs = list(map(lambda x: rune_dist(cur_rune, x), range(len(runes))))
    switch_costs = list(map(lambda x: sym_dist(x, c), runes))
    total_costs = [x + y for i, (x, y) in enumerate(zip(move_costs, switch_costs))]
    min_cost = min(total_costs)
    min_indices = [i for i, c in enumerate(total_costs) if c == min_cost]
    next_rune = min(min_indices, key=lambda x: abs(cur_rune - x))
    action = move_rune(next_rune)
    action += move_sym(next_rune, c)
    assert (len(action) == min_cost)
    action += '.'
    return action


def update_state(action):
    global cur_rune, runes
    for a in action:
        if a == '>':
            cur_rune += 1
        elif a == '<':
            cur_rune -= 1
        elif a == '+':
            runes[cur_rune] += 1
        elif a == '-':
            runes[cur_rune] -= 1
        elif a == '.':
            return lookup[runes[cur_rune]]
        if cur_rune > 29:
            cur_rune = 0
        if cur_rune < 0:
            cur_rune = 29
        if runes[cur_rune] > 26:
            runes[cur_rune] = 0
        if runes[cur_rune] < 0:
            runes[cur_rune] = 26


def get_actions(p):
    global actions, cur_rune, runes
    runes = list(itertools.repeat(0, 30))
    cur_rune = 0
    actions = ''
    update_state(actions)
    for i, c in enumerate(p):
        action = next_action(c)
        exp_c = update_state(action)
        assert exp_c == c
        actions += action
    return actions


if __name__ == '__main__':
    magic_phrase = input()
    total = 0
    print(get_actions(magic_phrase))
