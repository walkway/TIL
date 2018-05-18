# TDD bowiling

- 볼링 게임 TDD 따라하기

````
import org.junit.Before;
import org.junit.Test;

import static org.hamcrest.core.Is.is;
import static org.junit.Assert.assertThat;

public class GameTest {

    private Game game;

    @Before
    public void setUp() throws Exception {
        game = new Game();
    }

    private void rollMany(int rolls, int pins) {
        for(int i = 0; i < rolls; i++)
            game.roll(pins);
    }

    @Test
    public void canRoll() {
        game.roll(0);
    }

    @Test
    public void gutterGame() {
        rollMany(20, 0);
        assertThat(game.score(), is(0));
    }

    @Test
    public void allOnes() {
        rollMany(20, 1);
        assertThat(game.score(), is(20));
    }

    @Test
    public void oneSpare() {
        rollSpare();
        game.roll(3);
        rollMany(17, 0);
        assertThat(game.score(), is(16));
    }

    private void rollSpare() {
        game.roll(5);
        game.roll(5);
    }

    @Test
    public void oneStrike() {
        rollStrike();
        game.roll(5);
        game.roll(3);
        rollMany(16, 0);
        assertThat(game.score(), is(26));
    }

    private void rollStrike() {
        game.roll(10);
    }

    @Test
    public void perfectGame() {
        rollMany(12, 10);
        assertThat(game.score(), is(300));
    }
}
````
````
public class Game {
    private int[] rolls = new int[21];
    private int currentRoll = 0;

    public void roll(int pins) {
        rolls[currentRoll++] = pins;
    }

    public Integer score() {
        int score = 0;
        int firstFrame = 0;
        for(int frame = 0; frame < 10; frame++) {
            if(isStrike(firstFrame)) {
                score += 10 + nextTwoBallsForStrike(firstFrame);
                firstFrame += 1;
            }
            else if(isSpare(firstFrame)) {
                score += 10 + nextBallForSpare(firstFrame);
                firstFrame += 2;
            }
            else {
                score += nextBallsInFrame(firstFrame);
                firstFrame += 2;
            }
        }
        return score;
    }

    private int nextBallsInFrame(int firstFrame) {
        return rolls[firstFrame] + rolls[firstFrame + 1];
    }

    private int nextBallForSpare(int firstFrame) {
        return rolls[firstFrame + 2];
    }

    private int nextTwoBallsForStrike(int firstFrame) {
        return rolls[firstFrame + 1] + rolls[firstFrame + 2];
    }

    private boolean isStrike(int firstFrame) {
        return rolls[firstFrame] == 10;
    }

    private boolean isSpare(int firstFrame) {
        return rolls[firstFrame] + rolls[firstFrame + 1] == 10;
    }
}
````
https://youtu.be/fFwDMzML7hI