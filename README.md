# 30716
class Player:
    def __init__(self, name, team):
        self.name = name
        self.team = team

class Queue:
    def __init__(self, max_size):
        self.queue = []
        self.max_size = max_size

    def enqueue(self, item):
        if len(self.queue) < self.max_size:
            self.queue.append(item)
        else:
            print("대기열이 가득 찼습니다.")

    def dequeue(self):
        if not self.is_empty():
            return self.queue.pop(0)
        else:
            print("대기열이 비어있습니다.")

    def is_empty(self):
        return len(self.queue) == 0

    def is_full(self):
        return len(self.queue) == self.max_size

    def size(self):
        return len(self.queue)

class GameMatchmaking:
    def __init__(self, max_queue_size):
        self.queue = Queue(max_queue_size)
        self.team1 = []
        self.team2 = []

    def add_player(self, player):
        if player.team == "Team1":
            if len(self.team1) < 2:
                self.team1.append(player)
                print(f"{player.name}({player.team})이(가) 팀 1에 추가되었습니다.")
            else:
                print("팀 1이 가득 찼습니다.")
                if self.ask_for_queue_extension():
                    self.expand_queue()
                    self.team1.append(player)
                    print(f"{player.name}({player.team})이(가) 팀 1에 추가되었습니다.")
                else:
                    print("대기열이 확장되지 않았습니다.")
                return
        elif player.team == "Team2":
            if len(self.team2) < 2:
                self.team2.append(player)
                print(f"{player.name}({player.team})이(가) 팀 2에 추가되었습니다.")
            else:
                print("팀 2가 가득 찼습니다.")
                if self.ask_for_queue_extension():
                    self.expand_queue()
                    self.team2.append(player)
                    print(f"{player.name}({player.team})이(가) 팀 2에 추가되었습니다.")
                else:
                    print("대기열이 확장되지 않았습니다.")
                return
        else:
            print("올바른 팀을 입력하세요.")
            return

        self.queue.enqueue(player)

        if len(self.team1) >= 2 and len(self.team2) >= 2:
            self.match_players()

    def print_queue_status(self):
        print("대기열 상태:")
        print("팀 1 플레이어:")
        for player in self.team1:
            print(f"{player.name}({player.team})")
        print("팀 2 플레이어:")
        for player in self.team2:
            print(f"{player.name}({player.team})")
        print(f"대기열 플레이어 수: {self.queue.size()}")
        print(f"팀 1 플레이어 수: {len(self.team1)}")
        print(f"팀 2 플레이어 수: {len(self.team2)}")

    def match_players(self):
        print("매칭 결과:")
        for i in range(2):
            print(f"{self.team1[i].name}({self.team1[i].team}) vs {self.team2[i].name}({self.team2[i].team})")
        print("게임을 시작합니다.")

        self.team1 = self.team1[2:]
        self.team2 = self.team2[2:]

    def expand_queue(self):
        self.queue.max_size += 2
        print("대기열이 확장되었습니다.")

    @staticmethod
    def ask_for_queue_extension():
        response = input("대기열을 확장하시겠습니까? (y/n): ")
        return response.lower() == "y"


matchmaking = GameMatchmaking(max_queue_size=4)

while True:
    player_name = input("플레이어 이름을 입력하세요 (종료하려면 'q' 입력): ")

    if player_name == 'q':
        break

    player_team = input("팀을 입력하세요 (Team1 또는 Team2): ")

    player = Player(player_name, player_team)
    matchmaking.add_player(player)
    matchmaking.print_queue_status()
