# Game-2048-with-Python
		 [Win]--------------
		   ^Win	            |Exit
         init()    |	   Exit	    ~
[Init]  ------>  [Game]  ----->   [Exit]
	<------	   |GameOver	    ^Exit		
	 Restart   ~		    |	
		 [Gameover] ---------
 
state 存储当前状态，state_actions 这个词典变量作为状态转换的规则，它的Key是状态，value是返回下一个状态的函数：
	.Init:init()
		。Game
	.Game:game()
		。Game
		。Win
		。GameOver
		。Exit
	.Win:lambda:not_game('Win')
		。Init
		。Exit
	.Gameover:lambda:not_game("Gameover")
		。Init
		。Exit
	.Exit:退出循环
状态机不断循环，直到达到Exit终结状态结束程序

该程序只写了左移实现：右，上，下移通过矩阵转置与矩阵逆转来实现：
矩阵转置：
def transpose(field):
	return [list(row) for row in zip(*field)]

矩阵逆转（非逆矩阵):
def invert(field):
	return [row[::-1] for row in field]

随机生成一个2或者4
def spawn(self):
	pass

单行向左合并：
def move_row_left(low):
	def tighten(row): #把零散的非零单元挤到一块
		pass
	def merge(row):   #对相等的相邻元素进行合并
		pass
	return tighten(merge(tighten(row))) #先挤到一块再合并最后再挤到一块

棋盘走一步：
def move(self,,direction):
	def move_row_left(row):
		pass

判断输赢：
def is_win(self):
	pass
def is_gameover(self):
	pass

判断能否移动：
def move_is_possible(self,direction):
	def row_is_left_movable(row):
		def change(i):
			pass
完成主逻辑：
def main(stdscr):
    def init():
        #重置游戏棋盘
        game_field.reset()
        return 'Game'

    def not_game(state):
        #画出 GameOver 或者 Win 的界面
        game_field.draw(stdscr)
        #读取用户输入得到action，判断是重启游戏还是结束游戏
        action = get_user_action(stdscr)
        responses = defaultdict(lambda: state) #默认是当前状态，没有行为就会一直在当前界面循环
        responses['Restart'], responses['Exit'] = 'Init', 'Exit' #对应不同的行为转换到不同的状态
        return responses[action]

    def game():
        #画出当前棋盘状态
        game_field.draw(stdscr)
        #读取用户输入得到action
        action = get_user_action(stdscr)

        if action == 'Restart':
            return 'Init'
        if action == 'Exit':
            return 'Exit'
        if game_field.move(action): # move successful
            if game_field.is_win():
                return 'Win'
            if game_field.is_gameover():
                return 'Gameover'
        return 'Game'


    state_actions = {
            'Init': init,
            'Win': lambda: not_game('Win'),
            'Gameover': lambda: not_game('Gameover'),
            'Game': game
        }

    curses.use_default_colors()
    game_field = GameField(win=32)


    state = 'Init'

    #状态机开始循环
    while state != 'Exit':
        state = state_actions[state]()
