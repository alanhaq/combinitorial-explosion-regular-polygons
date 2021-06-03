import random
import itertools
import math

class Puzzle:
    def __init__(self, num=3, edges = [], puzzle = []):
        self.num = num
        self.edges = edges
        self.puzzle = puzzle
        
    def new_nut(self):
        x = random.randint(0, self.num)
        rand_list = random.sample(range(2,self.num+1), self.num-1)
        self.edges = self.edges + [1] + rand_list
        return self.edges
    
    def new_puzzle(self):
        a_puzzle = []
        self.puzzle = []
        while len(self.puzzle) <= self.num:
            potential_nut = Puzzle.new_nut(self)
            self.edges = []
            if len(self.puzzle) < 1:
                self.puzzle.append(potential_nut)
            elif self.num <= 3:
                self.puzzle.append(potential_nut)
            else:
                checker = True
                for item in self.puzzle:
                    if potential_nut == item:
                        checker = False
                if checker == True:
                    self.puzzle.append(potential_nut)
        a_puzzle = self.puzzle.copy()
        return a_puzzle
                    
    def main(self, puzzle_list):
        solution = []
        for permutation in itertools.permutations(puzzle_list):
            half_puzzle = list(permutation)
            center_nut = list(permutation[0])
            for x in range(0, self.num):
                while center_nut[x] != half_puzzle[x+1][0]:
                    half_puzzle[x+1] = Puzzle.rotate_nut(self, half_puzzle[x+1])
            if Puzzle.check_solution(self, half_puzzle):
                solution = str(half_puzzle)
                print('You found a solution!')
                print('****', half_puzzle)
                return (solution)
        if solution == []:
            print('There is no solution.')
            
    def main2(self, puzzle_test):
        solution = []
        solutions = 0
        puzzle_list = puzzle_test.copy()
        permutes = int(math.ceil(self.num/2)) + 1
        if self.num == 3:
            permutes = 4
        for permutation in itertools.permutations(puzzle_list, permutes):
            partial_puzzle = list(permutation)
            remaining_puzzle = []
                #remaining_puzzle gives the nuts that will tested to see if they fit in spaces
            for nut in puzzle_list:
                if not nut in partial_puzzle:
                    remaining_puzzle.append(nut)  
                #this gets every combo of 4 nuts
                #partial_puzzle gives the nuts that will be fixed to check if remaining nuts can fit 
                #first check to see if the fixed nuts work, if they do then continue
            if self.num == 3:
                remaining_puzzle = [partial_puzzle.pop(-1)]
            for sub_permutation in itertools.permutations(remaining_puzzle):
                other_puzzle = list(sub_permutation)
                if Puzzle.prune_check(self, partial_puzzle, other_puzzle):
                    solution.append(Puzzle.combine_puzzle(self, partial_puzzle, other_puzzle))
                    return solution
        if solution == []:
            print('There is no solution.')

    def brute_force(self, puzzle_list):
        solution = []
        solutions = 0
        for permutation in itertools.permutations(puzzle_list):
            half_puzzle = list(permutation)
            center_nut = list(permutation[0])
            for x in range(0, self.num):
                while center_nut[x] != half_puzzle[x+1][0]:
                    half_puzzle[x+1] = Puzzle.rotate_nut(self, half_puzzle[x+1])
            if Puzzle.check_solution(self, half_puzzle):
                solution.insert(-1, str(half_puzzle))
                solutions += 1
                #print('You found a solution!')
                #print('****', half_puzzle)
        if solution == []:
            #print('There is no solution.')
            return -1
        else:
            print('You found', solutions, 'solution/s')
            return solution  
        
    def prune(self, puzzle_test):
        solution = []
        solutions = 0
        puzzle_list = puzzle_test.copy()
        permutes = int(math.ceil(self.num/2)) + 1
        if self.num == 3:
            permutes = 4
        for permutation in itertools.permutations(puzzle_list, permutes):
            partial_puzzle = list(permutation)
            remaining_puzzle = []
                #remaining_puzzle gives the nuts that will tested to see if they fit in spaces
            for nut in puzzle_list:
                if not nut in partial_puzzle:
                    remaining_puzzle.append(nut)  
                #this gets every combo of 4 nuts
                #partial_puzzle gives the nuts that will be fixed to check if remaining nuts can fit 
                #first check to see if the fixed nuts work, if they do then continue
            if self.num == 3:
                remaining_puzzle = [partial_puzzle.pop(-1)]
            for sub_permutation in itertools.permutations(remaining_puzzle):
                other_puzzle = list(sub_permutation)
                if Puzzle.prune_check(self, partial_puzzle, other_puzzle):
                    solution.append(Puzzle.combine_puzzle(self, partial_puzzle, other_puzzle))
                    solutions += 1
                    #print('You found a solution!')
                    #print('***', solution[-1])
                    # this check will see if the remining pieces will fit
        if solution == []:
            #print('There is no solution.')
            return -1
        else:
            print('You found', solutions, 'solution/s')
            return solution
            
    def graph(self, n):
        test_puzzles = []
        zero_solutions = 0
        one_solutions = 0
        more_solutions = 0
        numbr = 0
        max = 0
        solution = []
        for x in range(0,n):
            Puzzle.new_puzzle(self)
            test_puzzles.insert(-1, self.puzzle.copy())
        for x in range(0,n):
            numbr = int(Puzzle.brute_force(self, test_puzzles[x]))
            if max < numbr:
                max = numbr
            if numbr == -1:
                zero_solutions += 1
            if numbr == 1:
                one_solutions += 1
            if numbr > 1:
                more_solutions += 1
        print('For', n, 'puzzles there are:', zero_solutions, 'with no solutions;', one_solutions, 'with one solution;', more_solutions, 'with more than 1 solution.')
        print('The most solutions found in a puzzle were', max)
        
    def rotate_nut(self, nut):
        n = 0
        new_nut = nut.copy()
        while n < 1:
            temp_num = new_nut.pop(0)
            new_nut.append(temp_num)
            n = n + 1
        return new_nut
        
    def check_solution(self, puzzle):
        puzzleR = puzzle.copy()
        for x in range(0, len(puzzleR)-1):
            if (x+2) > len(puzzleR)-1:
                if puzzleR[x+1][-1] != puzzleR[1][1]:
                    return False
            elif puzzleR[x+1][-1] != puzzleR[(x+2) % (self.num+1)][1]:
                return False
        return True
        
    def prune_check(self, puzzle1, puzzle2):
        puzzleA = puzzle1
        puzzleB = puzzle2
        n = 1
        for x in range(0, len(puzzleA)-1):
            while puzzleA[0][x*2] != puzzleA[x+1][0]:
                puzzleA[x+1] = Puzzle.rotate_nut(self, puzzleA[x+1])
            #rotate fixed nuts so inside edges match
        for x in range(1, len(puzzleB)+1):
            if len(puzzleB) == 1:
                while puzzleA[0][x] != puzzleB[0][0]:
                    puzzleB[0] = Puzzle.rotate_nut(self, puzzleB[0])
                break
            while puzzleA[0][x*2-1] != puzzleB[x-1][0]:
                puzzleB[x-1] = Puzzle.rotate_nut(self, puzzleB[x-1])
            #rotate other nuts so inside edges match
        if len(puzzleB) == 1 or len(puzzleA)-len(puzzleB) > 1:
            n = 2
        for x in range(0, len(puzzleA) - n):    #this will change depending on odd/even nuts (-2 or -1)
            if len(puzzleB) == 1:     #special case for 3
                if puzzleA[x+1][-1] != puzzleB[0][1]:
                    print('1')
                    return False
                if puzzleA[x+2][1] != puzzleB[0][-1]:
                    print('2')
                    return False
                if puzzleA[1][1] != puzzleA[x+2][-1]:
                    print('3')
                    return False
            elif x+2 >= len(puzzleA) and n==2:
                if puzzleA[1][1] != puzzleA[x+1][-1]:
                    return False
            elif x+2 >= len(puzzleA):
                if puzzleA[x+1][-1] != puzzleB[x][1]:
                    return False
                if puzzleA[1][1] != puzzleB[x][-1]:
                    return False
            else:
                if puzzleA[x+1][-1] != puzzleB[x][1]:
                    return False
                if puzzleA[x+2][1] != puzzleB[x][-1]:
                    return False
        return True
        
    def combine_puzzle(self, puzzle1 = [], puzzle2 = []):
        puzzleA = puzzle1.copy()
        puzzleB = puzzle2.copy()
        solution_nut = []
        solution_nut.append(puzzleA.pop(0))
        while puzzleA or puzzleB:
            if puzzleA:
                solution_nut.append(puzzleA.pop(0))
            if puzzleB:
                solution_nut.append(puzzleB.pop(0))
        return(str(solution_nut))
