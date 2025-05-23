#include <iostream>
#include <cmath>
#include <utility>
#include <vector>
using namespace std;

class Node {
public:
	int parent;
	pair<int, int> position;
	int g = 0;
	int f = 0;
	int h = 0;

	Node(int a, pair<int, int> po);  // parent, position 생성자
	double heuristic(const Node& node, const Node& goal, double D = 1.0, double D2 = std::sqrt(2.0));
   /* vector<int> aStar(vector<int> map, int start, int end);*/

    bool operator==(const Node& other) const {   // 파이썬에만 객체 같은지 아닌지 비교가 가능해서 따로 만들어야함
        if(position == other.position && parent == other.parent)
            return true;
        return false;
        // position이 pair<int,int>면 pair의 operator== 사용
    }
};

Node::Node(int a, pair<int, int> po): parent(a), position(po) {}

// 휴리스틱 계산용... 근데 어차피 안 쓸 것임....
double heuristic(const Node& node, const Node& goal, double D = 1.0, double D2 = std::sqrt(2.0)) {
	int dx = abs(node.position.first - goal.position.first);
	int dy = abs(node.position.second - goal.position.second);

	return D * (dx + dy) + (D2 - 2 * D) * min(dx, dy);
}

vector<int> aStar(vector<int> map, pair<int,int> start, pair<int, int> end) {
    Node startNode = Node(NULL, start);
    Node endNode = Node(NULL, end);

    vector<Node>openList;
    vector<Node>closedList;

    openList.push_back(startNode);

    while (!openList.empty()){
        Node currentNode = openList[0];
        int currentIdx = 0;

        int max = openList.size();
        for (int i = 0;i < max;i++) {
            if (openList[i].f < currentNode.f) {
                currentNode = openList[i];
                currentIdx = i;
            }
        }
        openList.erase(openList.begin() + currentIdx);
        closedList.push_back(currentNode);

        if (currentNode == endNode) {
            vector<pair<int,int>>path;
            Node current = currentNode; // 포인터로 참조
            while (current!= nullptr) {
                path.push_back(current->position);
            }

        }


    }
}

int main() {

    vector<int>maze;
    
    maze = [
        [0, 0, 0, 0, 1, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 1, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 1, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 1, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 1, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 1, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 1, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 1, 0, 0, 0, 0, 0],
            [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
    ];

    int start = (0, 0);
        end = (7, 6)

        path = aStar(maze, start, end)
        print("최단 경로:", path)
}

-----------



        if (currentNode == endNode) {
            // 경로 추적
            vector<pair<int, int>> path;
            int currentIndex = closedList.size() - 1; // 마지막 노드부터
            while (currentIndex != -1) {
                Node& current = closedList[currentIndex];
                path.push_back(current.position);
                currentIndex = current.parent;
            }

            return path;
        }
